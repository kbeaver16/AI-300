# DAY 07 — MODULE 5
Perform hyperparameter tuning with Azure Machine Learning

## MODULE LINK
- https://learn.microsoft.com/en-us/training/modules/perform-hyperparameter-tuning-azure-machine-learning-pipelines/

## TASKS
- [ ] Complete module
- [ ] Learn sweep jobs 
- [ ] Study tuning strategies

## KEY CONCEPTS
- Random vs grid vs Bayesian search
- Early stopping

## NOTES
# 📘 Hyperparameter Tuning with Azure Machine Learning

**Source:** [MS Learn Module](https://learn.microsoft.com/en-us/training/modules/perform-hyperparameter-tuning-azure-machine-learning-pipelines/) | Level: Beginner | Duration: ~46 min | 8 Units

---

## 📑 Table of Contents

1. [Introduction](https://copilot.microsoft.com/tasks/Yn5xbhAiDpDGv66sELT8M#introduction)
2. [Unit 2 — Define a Search Space](https://copilot.microsoft.com/tasks/Yn5xbhAiDpDGv66sELT8M#unit-2)
3. [Unit 3 — Configure a Sampling Method](https://copilot.microsoft.com/tasks/Yn5xbhAiDpDGv66sELT8M#unit-3)
4. [Unit 4 — Configure Early Termination](https://copilot.microsoft.com/tasks/Yn5xbhAiDpDGv66sELT8M#unit-4)
5. [Unit 5 — Use a Sweep Job for Hyperparameter Tuning](https://copilot.microsoft.com/tasks/Yn5xbhAiDpDGv66sELT8M#unit-5)
6. [Unit 6 — Exercise: Run a Sweep Job](https://copilot.microsoft.com/tasks/Yn5xbhAiDpDGv66sELT8M#unit-6)
7. [Vocabulary](https://copilot.microsoft.com/tasks/Yn5xbhAiDpDGv66sELT8M#vocabulary)
8. [Quick Reference Cheat Sheet](https://copilot.microsoft.com/tasks/Yn5xbhAiDpDGv66sELT8M#cheatsheet)

---

## 🔹 Unit 1 — Introduction (3 min) <a name="introduction"></a>

### What is a Hyperparameter?

- ML models learn **parameters** from training data (e.g., feature weights).
- **Hyperparameters** are configuration values set _before_ training begins — they are **not** learned from data. They control _how_ the model trains.
- The term exists because "parameter" is already taken by learned model values.

### Why Hyperparameter Tuning Matters

- Different hyperparameter values produce different models with different performance.
- Finding the optimal values can significantly improve accuracy and generalizability.
- The process involves training **multiple models** with the same algorithm and data, but different hyperparameter values, then comparing them.

### Examples of Hyperparameters by Algorithm

|Algorithm|Hyperparameter|Purpose|
|---|---|---|
|Logistic Regression|Regularization rate|Counteract overfitting|
|CNN (Deep Learning)|Learning rate|Control weight adjustment during training|
|CNN (Deep Learning)|Batch size|Number of samples per training batch|

### How Azure ML Handles This

- Submit a script as a **sweep job** in Azure Machine Learning.
- A sweep job runs a **trial** for each hyperparameter combination tested.
- Each trial trains a model and logs a **target performance metric** (e.g., accuracy).
- The best-performing trial's model is selected.

---

## 🔹 Unit 2 — Define a Search Space (5 min) <a name="unit-2"></a>

The **search space** is the full set of hyperparameter values that Azure ML will try during a sweep job. The type of value (discrete vs. continuous) determines which expressions you use.

---

### Discrete Hyperparameters

Values must come from a **finite set** of options.

#### `Choice` — Explicit list of values

```python
from azure.ai.ml.sweep import Choice, Normal

command_job_for_sweep = job(
    batch_size=Choice(values=[16, 32, 64]),         # list
    # OR: Choice(values=range(1, 10))               # range
    # OR: Choice(values=(30, 50, 100))              # tuple
)
```

#### Discrete Distribution Functions

|Function|Formula|Use Case|
|---|---|---|
|`QUniform(min, max, q)`|`round(Uniform(min, max) / q) * q`|Evenly spaced discrete values|
|`QLogUniform(min, max, q)`|`round(exp(Uniform(min, max)) / q) * q`|Logarithmically spaced discrete values|
|`QNormal(mu, sigma, q)`|`round(Normal(mu, sigma) / q) * q`|Normally distributed discrete values|
|`QLogNormal(mu, sigma, q)`|`round(exp(Normal(mu, sigma)) / q) * q`|Log-normally distributed discrete values|

---

### Continuous Hyperparameters

Values can be **any real number** along a scale (infinite possibilities).

#### Continuous Distribution Functions

|Function|Returns|Use Case|
|---|---|---|
|`Uniform(min, max)`|Uniformly distributed value between min and max|When all values equally likely|
|`LogUniform(min, max)`|`exp(Uniform(min, max))` — log of return is uniform|Wide ranges spanning orders of magnitude|
|`Normal(mu, sigma)`|Normally distributed real value|When a central value is most likely|
|`LogNormal(mu, sigma)`|`exp(Normal(mu, sigma))` — log of return is normal|Skewed distributions anchored near a center|

---

### Defining a Search Space — Full Example

```python
from azure.ai.ml.sweep import Choice, Normal

command_job_for_sweep = job(
    batch_size=Choice(values=[16, 32, 64]),  # discrete: one of 3 values
    learning_rate=Normal(mu=10, sigma=3),    # continuous: normal distribution
)
```

> **What this does:** `batch_size` will be sampled from {16, 32, 64}. `learning_rate` will be drawn from a normal distribution centered at 10 with std dev 3.

---

## 🔹 Unit 3 — Configure a Sampling Method (8 min) <a name="unit-3"></a>

Once the search space is defined, you must choose **how** Azure ML picks values from it. This is the **sampling algorithm**.

---

### The 3 Main Sampling Methods

|Method|Description|Works With|Best For|
|---|---|---|---|
|**Grid Sampling**|Tries every possible combination|Discrete only|Small, exhaustive search spaces|
|**Random Sampling**|Randomly picks values|Discrete + Continuous|Fast exploration of large spaces|
|**Bayesian Sampling**|Picks based on previous results|Choice, Uniform, QUniform|Efficiency when each trial is expensive|

---

### 1. Grid Sampling

Exhaustively tests **every combination** of discrete hyperparameters.

```python
from azure.ai.ml.sweep import Choice

command_job_for_sweep = command_job(
    batch_size=Choice(values=[16, 32, 64]),
    learning_rate=Choice(values=[0.01, 0.1, 1.0]),
)

sweep_job = command_job_for_sweep.sweep(
    sampling_algorithm="grid",
    ...
)
```

> **Example:** 3 batch sizes × 3 learning rates = **9 total trials**. All 9 will run.

---

### 2. Random Sampling

Randomly selects values from the search space. Can mix discrete and continuous.

```python
from azure.ai.ml.sweep import Normal, Uniform

command_job_for_sweep = command_job(
    batch_size=Choice(values=[16, 32, 64]),
    learning_rate=Normal(mu=10, sigma=3),
)

sweep_job = command_job_for_sweep.sweep(
    sampling_algorithm="random",
    ...
)
```

#### Sobol (Reproducible Random Sampling)

A variation of random sampling that uses a **seed** to make results **reproducible**. Also spreads distribution more evenly across the search space.

```python
from azure.ai.ml.sweep import RandomSamplingAlgorithm

sweep_job = command_job_for_sweep.sweep(
    sampling_algorithm=RandomSamplingAlgorithm(seed=123, rule="sobol"),
    ...
)
```

> **When to use:** When you need to reproduce a random sweep experiment exactly.

---

### 3. Bayesian Sampling

Uses the **Bayesian optimization algorithm** — selects new values based on how well _previous_ trials performed. Learns as it goes.

```python
from azure.ai.ml.sweep import Uniform, Choice

command_job_for_sweep = job(
    batch_size=Choice(values=[16, 32, 64]),
    learning_rate=Uniform(min_value=0.05, max_value=0.1),
)

sweep_job = command_job_for_sweep.sweep(
    sampling_algorithm="bayesian",
    ...
)
```

> ⚠️ **Constraint:** Bayesian sampling only works with `choice`, `uniform`, and `quniform` parameter expressions.

---

## 🔹 Unit 4 — Configure Early Termination (8 min) <a name="unit-4"></a>

Early termination stops a sweep job when new trials are no longer producing meaningfully better results — saving compute time and cost.

### When to Use Early Termination

|Scenario|Recommendation|
|---|---|
|Small discrete search space (e.g., 6 grid trials)|Early termination likely **unnecessary**|
|Continuous hyperparameters + Random/Bayesian sampling|Early termination **highly recommended**|

---

### Key Configuration Parameters

|Parameter|Description|
|---|---|
|`evaluation_interval`|How often (in logged metric intervals) to evaluate the policy|
|`delay_evaluation`|Number of initial trials to skip before the policy starts — ensures a baseline of completed trials|

---

### Early Termination Policy Types

#### 1. 🏦 Bandit Policy

Terminates runs where the metric underperforms the best trial by more than a specified **slack** amount.

- `slack_amount` — absolute difference allowed
- `slack_factor` — relative (ratio) difference allowed

```python
from azure.ai.ml.sweep import BanditPolicy

sweep_job.early_termination = BanditPolicy(
    slack_amount=0.2,        # absolute slack
    delay_evaluation=5,      # skip first 5 trials
    evaluation_interval=1    # evaluate every trial after
)
```

> **Example:** Best model so far has accuracy 0.9. Slack = 0.2. New model must score > (0.9 − 0.2) = **0.7** to continue. If it scores 0.65, the sweep stops.

---

#### 2. 📊 Median Stopping Policy

Terminates trials where the metric is **worse than the running median** of all trials so far.

```python
from azure.ai.ml.sweep import MedianStoppingPolicy

sweep_job.early_termination = MedianStoppingPolicy(
    delay_evaluation=5,
    evaluation_interval=1
)
```

> **Example:** After 5 trials, the median accuracy is 0.82. Trial 6 must score > **0.82** or the sweep stops.

---

#### 3. ✂️ Truncation Selection Policy

Cancels the **lowest-performing X%** of trials at each evaluation interval.

```python
from azure.ai.ml.sweep import TruncationSelectionPolicy

sweep_job.early_termination = TruncationSelectionPolicy(
    evaluation_interval=1,
    truncation_percentage=20,  # cancel lowest 20% of trials
    delay_evaluation=4
)
```

> **Example:** After 4 trials, the 5th trial must NOT be in the worst 20% (i.e., worst 1 out of 5). If it is the worst performer, the sweep stops.

---

### Policy Comparison at a Glance

|Policy|Key Parameter|Decision Logic|
|---|---|---|
|Bandit|`slack_amount` / `slack_factor`|Must be within slack of best trial|
|Median Stopping|_(none beyond intervals)_|Must beat the running median|
|Truncation Selection|`truncation_percentage`|Must not be in the bottom X%|

---

## 🔹 Unit 5 — Use a Sweep Job for Hyperparameter Tuning (8 min) <a name="unit-5"></a>

### Step 1: Create a Training Script

Your script **must**:

1. Accept each hyperparameter as a **command-line argument** (`argparse`)
2. **Log the target metric** using **MLflow**

```python
import argparse
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LogisticRegression
import mlflow

# 1. Parse the hyperparameter argument
parser = argparse.ArgumentParser()
parser.add_argument('--regularization', type=float, dest='reg_rate', default=0.01)
args = parser.parse_args()
reg = args.reg_rate

# 2. Load data
data = pd.read_csv("data.csv")
X = data[['feature1','feature2','feature3','feature4']].values
y = data['label'].values
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.30)

# 3. Train model with the hyperparameter
model = LogisticRegression(C=1/reg, solver="liblinear").fit(X_train, y_train)

# 4. Log the metric (MLflow)
y_hat = model.predict(X_test)
acc = np.average(y_hat == y_test)
mlflow.log_metric("Accuracy", acc)
```

---

### Step 2: Create a Base Command Job

```python
from azure.ai.ml import command

job = command(
    code="./src",
    command="python train.py --regularization ${{inputs.reg_rate}}",
    inputs={"reg_rate": 0.01},
    environment="AzureML-sklearn-0.24-ubuntu18.04-py37-cpu@latest",
    compute="aml-cluster",
)
```

### Step 3: Override Inputs with the Search Space

```python
from azure.ai.ml.sweep import Choice

command_job_for_sweep = job(
    reg_rate=Choice(values=[0.01, 0.1, 1]),
)
```

### Step 4: Call `.sweep()` to Create and Configure the Sweep Job

```python
from azure.ai.ml import MLClient

sweep_job = command_job_for_sweep.sweep(
    compute="aml-cluster",
    sampling_algorithm="grid",
    primary_metric="Accuracy",     # metric logged by MLflow in the script
    goal="Maximize",               # or "Minimize"
)

sweep_job.experiment_name = "sweep-example"

# Set limits
sweep_job.set_limits(
    max_total_trials=4,            # max total trial runs
    max_concurrent_trials=2,       # max trials running at once
    timeout=7200                   # max seconds for entire sweep
)

# Submit
returned_sweep_job = ml_client.create_or_update(sweep_job)
```

---

### Monitoring Sweep Jobs

- View in **Azure Machine Learning Studio**
- Each trial's logged metrics are visible individually
- Charts let you **compare hyperparameter values vs. performance metrics** across all trials

---

## 🔹 Unit 6 — Exercise: Run a Sweep Job (10 min) <a name="unit-6"></a>

**Hands-on Lab Goals:**

1. Run a **command job** (baseline training run)
2. Configure a **sweep job** using the command job as a base
3. **Submit** the sweep job and observe results

**Lab Link:** [mslearn-azure-ml — 09-Hyperparameter-tuning](https://microsoftlearning.github.io/mslearn-azure-ml/Instructions/09-Hyperparameter-tuning.html)

---

## 📖 Vocabulary <a name="vocabulary"></a>

|Term|Definition|
|---|---|
|**Hyperparameter**|A configuration value set _before_ training begins; not learned from data. Contrasts with model _parameters_ which are learned during training.|
|**Parameter**|A value _learned_ from training data (e.g., feature weights in a model).|
|**Hyperparameter Tuning**|The process of training multiple models with different hyperparameter values to find the best-performing combination.|
|**Search Space**|The defined range or set of possible values for each hyperparameter to be tested during a sweep.|
|**Sweep Job**|An Azure ML job that runs multiple trials, each using a different hyperparameter combination from the search space.|
|**Trial**|A single training run within a sweep job, using one specific combination of hyperparameter values.|
|**Sampling Algorithm**|The method used to select hyperparameter value combinations from the search space (Grid, Random, Bayesian).|
|**Grid Sampling**|Exhaustively tests every possible combination of discrete hyperparameters.|
|**Random Sampling**|Randomly selects values from the search space; works with both discrete and continuous values.|
|**Sobol**|A reproducible variant of random sampling that uses a seed and distributes values more evenly.|
|**Bayesian Sampling**|An intelligent sampling method that selects new values based on the performance of previous trials.|
|**Early Termination Policy**|A rule that automatically stops the sweep job when new trials are unlikely to improve upon the best result.|
|**Bandit Policy**|Early termination that stops runs performing outside a defined slack from the best trial.|
|**Median Stopping Policy**|Early termination that stops runs performing below the running median of all trials.|
|**Truncation Selection Policy**|Early termination that cancels the lowest-performing X% of trials at each evaluation interval.|
|**slack_amount**|Absolute performance gap allowed from the best trial in a Bandit policy.|
|**slack_factor**|Relative (ratio) performance gap allowed from the best trial in a Bandit policy.|
|**evaluation_interval**|How frequently (in metric-log intervals) an early termination policy is checked.|
|**delay_evaluation**|Minimum number of trials to complete before the early termination policy starts checking.|
|**primary_metric**|The performance metric logged by MLflow that the sweep job uses to rank trials.|
|**goal**|Whether to `Maximize` or `Minimize` the primary metric.|
|**max_total_trials**|The maximum number of trial runs allowed in the entire sweep job.|
|**max_concurrent_trials**|The maximum number of trials that can run in parallel at once.|
|**MLflow**|An open-source tracking framework used to log metrics from training scripts so Azure ML can evaluate sweep trials.|
|**Command Job**|A base job in Azure ML that runs a script; used as the foundation for creating a sweep job.|
|**Regularization Rate**|A hyperparameter used in logistic regression to penalize overly complex models and reduce overfitting.|
|**Overfitting**|When a model learns training data too precisely and performs poorly on new, unseen data.|
|**Discrete Hyperparameter**|A hyperparameter that takes values from a finite set (e.g., batch size of 16, 32, or 64).|
|**Continuous Hyperparameter**|A hyperparameter that can be any real number within a range (e.g., learning rate between 0.001 and 1.0).|
|**QUniform / QNormal**|Quantized distributions — produce discrete values by rounding continuous distributions to multiples of `q`.|

---

## ⚡ Quick Reference Cheat Sheet <a name="cheatsheet"></a>

```
SWEEP JOB WORKFLOW
─────────────────────────────────────────────────────
1. Write training script
   └── argparse for hyperparams  +  mlflow.log_metric()

2. Define search space
   ├── Discrete: Choice([...]), QUniform, QNormal
   └── Continuous: Uniform, Normal, LogUniform, LogNormal

3. Choose sampling method
   ├── "grid"      → all combos (discrete only)
   ├── "random"    → random picks (any type)
   ├── Sobol       → reproducible random (add seed)
   └── "bayesian"  → learns from results (choice/uniform/quniform only)

4. (Optional) Add early termination policy
   ├── BanditPolicy        → slack from best trial
   ├── MedianStoppingPolicy → must beat median
   └── TruncationSelectionPolicy → can't be in bottom X%

5. Configure and submit sweep job
   └── set primary_metric, goal, max_total_trials,
       max_concurrent_trials, timeout
─────────────────────────────────────────────────────
```