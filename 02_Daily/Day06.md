# DAY 06 — MODULE 4
Experiment with Azure Machine Learning

## MODULE LINK
- https://learn.microsoft.com/en-us/training/paths/automate-machine-learning-model-selection-azure-machine-learning/

## TASKS
- [ ] Complete module 
- [ ] Review MLflow tracking 
- [ ] Use AutoML

## KEY CONCEPTS
- Experiment tracking
- Responsible AI dashboard

## NOTES
# 📘 Experiment with Azure Machine Learning — Full Learning Path Notes

**Source:** Microsoft Learn | Level: Beginner | Role: Data Scientist | Total Time: ~1 hr 10 min
**Learning Path:** [Automate Machine Learning Model Selection with Azure Machine Learning](https://learn.microsoft.com/en-us/training/paths/automate-machine-learning-model-selection-azure-machine-learning/)

---

## 📌 Learning Path Overview

This learning path contains **2 modules** covering two complementary approaches to finding the best ML model:

| Module | Topic | Time | Units |
|---|---|---|---|
| 1 | Find the best classification model with Automated Machine Learning (AutoML) | 37 min | 7 |
| 2 | Track model training in Jupyter notebooks with MLflow | 33 min | 6 |

**Core Goal:** Learn how to avoid manual trial-and-error in ML model development — either by fully automating model selection (AutoML) or by systematically tracking experiments (MLflow).

---

---

# 🧩 MODULE 1: Find the Best Classification Model with Automated Machine Learning

---

## Unit 1 — Introduction

**Problem AutoML Solves:** Going through trial and error to find the best-performing machine learning model is extremely time-consuming. Manually testing and evaluating various configurations — preprocessing steps, algorithm choices, hyperparameters — is tedious and error-prone.

**Solution — AutoML:** AutoML (Automated Machine Learning) automates the process of trying multiple preprocessing transformations and algorithms against your data, then surfaces the best-performing model based on a metric you define.

**Conceptual Flow:**
```
Training Data → AutoML Engine → Trains Multiple Models → Evaluates Each → Returns Best Model
```

**AutoML Interfaces in Azure Machine Learning:**
- **Azure Machine Learning Studio** — visual, no-code interface
- **Azure CLI** — command-line interface
- **Python SDK (v2)** — programmatic control; preferred by data scientists

> ⚠️ **Note:** While this module focuses on **classification**, AutoML also supports:
> - Regression
> - Forecasting (time series)
> - Image classification
> - Natural language processing (NLP)

**Module Learning Objectives:**
1. Prepare data to use AutoML for classification
2. Configure and run an AutoML experiment
3. Evaluate and compare models

---

## Unit 2 — Preprocess Data and Configure Featurization

### Step 1: Prepare Your Data

Before running AutoML, you must supply training data in a format Azure Machine Learning understands. For classification tasks, **only training data is required** (no separate test set is mandatory upfront).

### Step 2: Create a Data Asset

You must register your data as a **data asset** in Azure Machine Learning. AutoML requires a special type of data asset called an **MLTable**.

**What is an MLTable?**
An MLTable data asset includes the **schema of the data** (column names, types, etc.) alongside the actual data files. The data and a special `MLTable` definition file must be stored together in a folder.

**📌 Example — Referencing a MLTable as Input:**
```python
from azure.ai.ml.constants import AssetTypes
from azure.ai.ml import Input

my_training_data_input = Input(
    type=AssetTypes.MLTABLE,
    path="azureml:input-data-automl:1"   # references a registered data asset
)
```
- `AssetTypes.MLTABLE` — specifies the MLTable format
- `path` — points to the registered data asset by name and version (`name:version`)

---

### Step 3: Understand Featurization

**Featurization** is the automatic preprocessing of your data before model training. AutoML performs this by default, but it is configurable.

#### 3a. Scaling & Normalization (Always Applied)
AutoML **automatically** applies scaling and normalization to numeric data. This prevents features with large ranges from dominating the training process. Multiple techniques are tried across different model candidates.

> **Example:** A dataset with an "Age" column (0–100) and an "Income" column (0–200,000) would have Income dominate without normalization. AutoML handles this automatically.

#### 3b. Optional Featurization Transformations
When enabled, AutoML can also apply:

| Transformation | Purpose |
|---|---|
| **Missing value imputation** | Fills null/missing values so training can proceed |
| **Categorical encoding** | Converts text categories to numeric indicators (e.g., one-hot encoding) |
| **High-cardinality feature dropping** | Removes columns like record IDs that have too many unique values to be useful |
| **Feature engineering** | Derives new features (e.g., extracts day, month, year from a DateTime column) |

**Featurization is ON by default.** You can:
- **Disable it** — if you've already preprocessed your data and don't want AutoML to transform it
- **Customize it** — specify which imputation method to use for specific columns

#### 3c. Post-Experiment Review
After AutoML finishes, you can review:
- Which scaling/normalization technique was applied to each model
- Any **data quality alerts** (e.g., missing values detected, class imbalance found)

---

## Unit 3 — Run an Automated Machine Learning Experiment

### Classification Algorithms AutoML Tries

When the task is classification, AutoML selects from this pool of algorithms:

| Algorithm | Notes |
|---|---|
| Logistic Regression | Linear baseline classifier |
| Light GBM (LightGBM) | Gradient boosting, fast and efficient |
| Decision Tree | Simple interpretable model |
| Random Forest | Ensemble of decision trees |
| Naive Bayes | Probabilistic classifier |
| Linear SVM (Support Vector Machine) | Effective for high-dimensional data |
| XGBoost | High-performance gradient boosting |
| ...and others | Full list in Azure docs |

> **Tip:** You can **block specific algorithms** from being selected — useful if your data is known to be unsuitable for certain approaches, or if your organization has a policy restricting specific algorithm types.

---

### Configuring an AutoML Experiment with Python SDK v2

The main class is `automl` from `azure.ai.ml`. For classification, use `automl.classification()`.

**📌 Example — Basic AutoML Classification Job:**
```python
from azure.ai.ml import automl

classification_job = automl.classification(
    compute="aml-cluster",                    # compute cluster to run on
    experiment_name="auto-ml-class-dev",     # name for the experiment group
    training_data=my_training_data_input,    # MLTable data asset (from Unit 2)
    target_column_name="Diabetic",           # the label/output column
    primary_metric="accuracy",               # metric to optimize
    n_cross_validations=5,                   # number of cross-validation folds
    enable_model_explainability=True         # generate feature importance
)
```

---

### Specifying the Primary Metric

The **primary metric** is the key performance measure AutoML uses to rank and select the best model. You must pick one.

**📌 Example — Listing Available Classification Metrics:**
```python
from azure.ai.ml.automl import ClassificationPrimaryMetrics

list(ClassificationPrimaryMetrics)
```

Common classification primary metrics include:
- `accuracy` — overall % of correct predictions
- `AUC_weighted` — area under the ROC curve, weighted by class
- `norm_macro_recall` — normalized macro recall
- `average_precision_score_weighted` — precision-recall area
- `precision_score_weighted` — weighted precision

> **Key Decision:** Choose your metric based on your business problem. For imbalanced datasets (e.g., fraud detection), accuracy alone is misleading — prefer AUC or recall-based metrics.

---

### Setting Limits (Cost/Time Control)

Training many model candidates is expensive. Use `set_limits()` to cap time and compute usage.

**📌 Example — Setting Experiment Limits:**
```python
classification_job.set_limits(
    timeout_minutes=60,           # entire experiment stops after 60 minutes
    trial_timeout_minutes=20,     # any single model trial times out at 20 min
    max_trials=5,                 # train at most 5 different model candidates
    enable_early_termination=True # stop early if score isn't improving
)
```

| Parameter | Meaning |
|---|---|
| `timeout_minutes` | Total wall-clock limit for the entire AutoML job |
| `trial_timeout_minutes` | Max time for any single model trial |
| `max_trials` | Hard cap on number of models trained |
| `enable_early_termination` | Stops the job if short-term improvement plateaus |
| `max_concurrent_trials` | (Optional) Parallel trials — limited by compute cluster node count |

> **Tip on Parallelism:** If your compute cluster has 4 nodes, you can run up to 4 trials in parallel simultaneously. Use `max_concurrent_trials` to limit this if desired.

---

### Setting Training Properties

You can also configure which algorithms are included/excluded:
- **Block specific algorithms** (exclude ones you know won't work)
- **Allow/disallow ensemble models** (stacking/blending multiple models together)

---

### Submitting the AutoML Job

**📌 Example — Submitting the Job:**
```python
# Submit the AutoML job
returned_job = ml_client.jobs.create_or_update(
    classification_job
)
```

**📌 Example — Getting a Link to Monitor in Azure ML Studio:**
```python
aml_url = returned_job.studio_url
print("Monitor your job at", aml_url)
```
This prints a direct URL to the running job in Azure Machine Learning Studio where you can watch live progress.

---

## Unit 4 — Evaluate and Compare Models

### Reviewing Results in Azure ML Studio

After the experiment completes, navigate to the **AutoML experiment run** in the Azure Machine Learning Studio:

1. **Overview tab** — Shows the input data asset and the **best model summary**
2. **Models tab** — Lists every model trained, sorted by the primary metric (best at top)
   - You can **edit columns** to display multiple metrics side by side for richer comparison
   - The **Algorithm name** column shows both the **scaling method** and **algorithm** used (e.g., `MaxAbsScaler, LightGBM`)

---

### Data Guardrails

When featurization is enabled, AutoML also applies **data guardrails** — automatic checks for common data problems. Three guardrails apply to classification:

| Guardrail | What it Checks |
|---|---|
| **Class balancing detection** | Are classes heavily imbalanced? (e.g., 99% one class, 1% another) |
| **Missing feature values imputation** | Were missing values filled in? |
| **High cardinality feature detection** | Are there features with too many unique values? |

**Each guardrail reports one of three states:**

| State | Meaning | Action Needed |
|---|---|---|
| **Passed** | No problem detected | None |
| **Done** | AutoML made changes to your data | Review the changes AutoML applied |
| **Alerted** | Issue detected, but AutoML couldn't fix it | You must fix the data manually |

---

### Understanding Algorithm Names in Results

The algorithm name in the Models tab is composed of two parts:
```
MaxAbsScaler, LightGBM
    ↑ scaling technique    ↑ classification algorithm
```
- **MaxAbsScaler** = scaled each feature by its maximum absolute value
- **LightGBM** = the gradient boosting classifier used

---

### Model Explainability

You can generate explanations to understand **why** a model makes predictions:
- When configuring: set `enable_model_explainability=True` to auto-generate for the best model
- Anytime after: select any model in the Models tab → click **"Explain model"**

> **Important:** Explanations are **approximations** of model interpretability. They estimate the **relative importance of input features** in predicting the target column — they are not exact mathematical proofs.

> **Example:** If you're predicting diabetes, an explanation might show that "Blood Glucose Level" has 45% relative feature importance, while "Age" has 15%.

---

## Unit 7 — Summary (Module 1)

You can now:
- Prepare data as a **MLTable data asset** for AutoML
- Configure an AutoML classification job with `automl.classification()`, specifying compute, metrics, limits, and featurization options
- Submit the job and monitor it via Azure ML Studio
- Review, compare, and explain trained models

---
---

# 🧩 MODULE 2: Track Model Training in Jupyter Notebooks with MLflow

---

## Unit 1 — Introduction

### The Scenario

A data scientist at a cancer research lab needs to:
1. Train a model to detect breast cancer from tissue images
2. **Iteratively retrain** the model over time as new data arrives
3. Deploy improved versions into a researcher-facing application

The challenge: **How do you track each experiment run** so you can compare results, identify the best version, and reproduce successful training runs?

**Answer: MLflow**

### Module Learning Objectives
1. Configure MLflow to use in notebooks
2. Use MLflow for model tracking in notebooks

---

## Unit 2 — Configure MLflow for Model Tracking in Notebooks

### What is MLflow?

**MLflow** is an open-source library for tracking and managing machine learning experiments. It is framework-agnostic (works with scikit-learn, XGBoost, TensorFlow, PyTorch, etc.).

The key component used here is **MLflow Tracking** — it logs everything about a model training run, including:
- **Parameters** — inputs to the training process (hyperparameters, settings)
- **Metrics** — output measurements (accuracy, loss, AUC)
- **Artifacts** — files produced (model files, plots, images)

### Two Packages Required

| Package | Purpose |
|---|---|
| `mlflow` | Core open-source MLflow library |
| `azureml-mlflow` | Azure ML integration — connects MLflow to the AML workspace |

---

### Option A: Using Azure Machine Learning Notebooks (Easiest)

If you create and run notebooks **within the Azure Machine Learning workspace** on a **compute instance**, MLflow is **already configured** — no setup needed.

**📌 Example — Verify packages are installed:**
```python
pip show mlflow
pip show azureml-mlflow
```

---

### Option B: Using a Local Device

If you work locally (e.g., VS Code or local Jupyter), you must configure MLflow manually:

**Step-by-step:**
1. Install both packages:
```python
pip install mlflow
pip install azureml-mlflow
```
2. Open **Azure Machine Learning Studio**
3. Click your workspace name (top-right corner) → **"View all properties in Azure portal"**
4. Copy the **MLflow tracking URI** from the Azure portal overview page
5. Set it in your notebook:

**📌 Example — Set Tracking URI Locally:**
```python
mlflow.set_tracking_uri = "MLFLOW-TRACKING-URI"
# Replace with the URI copied from the Azure portal
```

> **What the Tracking URI does:** It tells MLflow where to send all logged data — in this case, your Azure Machine Learning workspace instead of the local filesystem.

---

## Unit 3 — Train and Track Models in Notebooks

### Concept: Experiments and Runs

MLflow organizes tracking with two levels:
- **Experiment** — a named group of related runs (e.g., all attempts to train a heart condition classifier)
- **Run** — a single execution of training code, tracked within an experiment

### Step 1: Create/Set an Experiment

**📌 Example:**
```python
import mlflow

mlflow.set_experiment(experiment_name="heart-condition-classifier")
```
- If the experiment doesn't exist, MLflow creates it
- If you skip this step, all runs go to a default experiment called `"Default"`

---

### Step 2: Start a Run and Log Results

Wrap your training code in `mlflow.start_run()` to begin tracking.

You have two logging approaches:

---

#### Approach A: Autologging (Recommended for Supported Frameworks)

MLflow can **automatically** detect what framework you're using and log all relevant parameters, metrics, artifacts, and the model itself — with one line.

**📌 Example — XGBoost with Autologging:**
```python
from xgboost import XGBClassifier

with mlflow.start_run():
    mlflow.xgboost.autolog()    # Enable autologging for XGBoost

    model = XGBClassifier(
        use_label_encoder=False,
        eval_metric="logloss"
    )
    model.fit(X_train, y_train,
              eval_set=[(X_test, y_test)],
              verbose=False)
```
As soon as `mlflow.xgboost.autolog()` is called, a run starts in the AML workspace and all XGBoost-relevant data is captured automatically.

**Supported autolog frameworks include:** scikit-learn, XGBoost, LightGBM, TensorFlow/Keras, PyTorch, Spark MLlib, and more (full list in MLflow docs).

---

#### Approach B: Custom Logging (Manual, Maximum Control)

Use when you need to log specific custom values not captured by autologging, or when your framework doesn't support autologging.

**Core Custom Logging Functions:**

| Function | What it Logs | When to Use |
|---|---|---|
| `mlflow.log_param(key, value)` | Single input parameter | Log hyperparameters, settings |
| `mlflow.log_metric(key, value)` | Single numeric output | Log accuracy, loss, AUC, F1, etc. |
| `mlflow.log_artifact(path)` | A file (image, CSV, etc.) | Log confusion matrix plots, feature importance charts |
| `mlflow.log_model(model, name)` | A trained model object | Create a versioned, deployable MLflow model |

**📌 Example — Custom Logging with XGBoost:**
```python
from xgboost import XGBClassifier
from sklearn.metrics import accuracy_score

with mlflow.start_run():
    model = XGBClassifier(
        use_label_encoder=False,
        eval_metric="logloss"
    )
    model.fit(X_train, y_train,
              eval_set=[(X_test, y_test)],
              verbose=False)

    y_pred = model.predict(X_test)
    accuracy = accuracy_score(y_test, y_pred)

    mlflow.log_metric("accuracy", accuracy)   # log the metric manually
```

> **Autologging vs. Custom Logging:** You can use them together. Autologging captures framework-level defaults; custom logging adds your own application-specific metrics on top.

---

### Viewing Results in Azure ML Studio

After any run completes, navigate to **Azure Machine Learning Studio → Experiments** to view:
- All logged **parameters** (training settings)
- All logged **metrics** (performance numbers)
- All logged **artifacts** (files, model packages)
- Comparison across multiple runs in the same experiment

---

## Unit 6 — Summary (Module 2)

You can now:
- Install and configure MLflow (`mlflow` + `azureml-mlflow`) for use in both AML notebooks and local notebooks
- Set a tracking URI to direct logs to your Azure ML workspace
- Create MLflow experiments to organize runs
- Use **autologging** for one-line tracking with supported frameworks
- Use **custom logging** with `log_param`, `log_metric`, `log_artifact`, and `log_model`
- Review all results in Azure ML Studio

---
---

# 📖 Vocabulary / Glossary

| Term | Definition |
|---|---|
| **AutoML (Automated Machine Learning)** | A capability that automatically tries multiple algorithms and preprocessing configurations to find the best ML model, eliminating manual trial-and-error |
| **Classification** | A supervised ML task where the model predicts a discrete category label (e.g., diabetic/not diabetic, spam/not spam) |
| **MLTable** | A special Azure ML data asset type that packages data files together with a schema definition file, required as input to AutoML jobs |
| **Data Asset** | A registered, versioned reference to a dataset stored in Azure Machine Learning, reusable across experiments |
| **Featurization** | Automated preprocessing of raw data before model training — includes scaling, encoding, imputation, and feature engineering |
| **Scaling / Normalization** | Transforming numeric feature values to a common range (e.g., 0–1) so that large-scale features don't dominate learning |
| **Missing Value Imputation** | Filling in null/missing values in a dataset using statistical methods (mean, median, mode, etc.) |
| **Categorical Encoding** | Converting text-based category values (e.g., "Yes"/"No") into numeric representations (e.g., 1/0) that algorithms can process |
| **High Cardinality** | A feature with many unique values relative to dataset size (e.g., a customer ID column), which is typically uninformative and dropped |
| **Feature Engineering** | Creating new, more informative features from existing ones (e.g., extracting "DayOfWeek" from a DateTime column) |
| **Primary Metric** | The single performance metric AutoML optimizes to rank and select the best model (e.g., accuracy, AUC, precision) |
| **Trial** | A single model training attempt within an AutoML experiment — one specific algorithm + featurization combination |
| **Early Termination** | Automatically stopping an AutoML experiment when scores stop improving, saving compute time |
| **Data Guardrail** | An automatic check AutoML performs to detect common data quality issues — reports Passed, Done, or Alerted |
| **Class Imbalance** | When one target class is much more prevalent than others in training data (e.g., 95% negative, 5% positive), causing biased models |
| **Model Explainability** | An approximation of which input features most influenced a model's predictions, expressed as relative feature importance |
| **MLflow** | An open-source platform for tracking, managing, and reproducing machine learning experiments |
| **MLflow Tracking** | The MLflow component that logs parameters, metrics, and artifacts from training runs |
| **Experiment (MLflow)** | A named container that groups related training runs together in MLflow |
| **Run (MLflow)** | A single execution of training code tracked by MLflow — logs parameters, metrics, and artifacts for one training attempt |
| **Parameter** | A configuration input to a training run — logged with `mlflow.log_param()` (e.g., learning rate, max depth) |
| **Metric** | A numeric output from a training run — logged with `mlflow.log_metric()` (e.g., accuracy = 0.94) |
| **Artifact** | A file output from a training run — logged with `mlflow.log_artifact()` (e.g., confusion matrix PNG, model file) |
| **Autologging** | An MLflow feature that automatically captures all relevant parameters, metrics, and artifacts for supported frameworks with a single function call |
| **Custom Logging** | Manually specifying what to log in MLflow using `log_param()`, `log_metric()`, `log_artifact()`, or `log_model()` |
| **Compute Instance** | A managed virtual machine in Azure ML Studio used to run notebooks interactively |
| **Compute Cluster** | A scalable pool of VMs in Azure ML used to run training jobs, supporting parallel trials |
| **MLflow Tracking URI** | A URI that tells MLflow where to store experiment data — set to your AML workspace to log results to Azure |
| **MaxAbsScaler** | A scaling technique that divides each feature value by the maximum absolute value of that feature, mapping values to [−1, 1] |
| **LightGBM** | Light Gradient Boosting Machine — a fast, distributed gradient boosting framework widely used for tabular classification |
| **XGBoost** | Extreme Gradient Boosting — another high-performance gradient boosting algorithm frequently used in AutoML |
| **Python SDK v2** | The second-generation Azure Machine Learning Python library (`azure-ai-ml`) used to configure and submit ML jobs programmatically |
| **Ensemble Model** | A model that combines predictions from multiple individual models (e.g., stacking, voting) for improved performance |

---

# 💡 Key Code Examples — Quick Reference

### AutoML: Configure & Submit
```python
from azure.ai.ml import automl
from azure.ai.ml.constants import AssetTypes
from azure.ai.ml import Input

# 1. Define input data
my_training_data_input = Input(type=AssetTypes.MLTABLE, path="azureml:input-data-automl:1")

# 2. Configure the AutoML job
classification_job = automl.classification(
    compute="aml-cluster",
    experiment_name="auto-ml-class-dev",
    training_data=my_training_data_input,
    target_column_name="Diabetic",
    primary_metric="accuracy",
    n_cross_validations=5,
    enable_model_explainability=True
)

# 3. Set limits
classification_job.set_limits(
    timeout_minutes=60,
    trial_timeout_minutes=20,
    max_trials=5,
    enable_early_termination=True
)

# 4. Submit
returned_job = ml_client.jobs.create_or_update(classification_job)
print("Monitor at:", returned_job.studio_url)
```

---

### MLflow: Set Up and Autolog
```python
import mlflow
from xgboost import XGBClassifier

# Set experiment
mlflow.set_experiment("heart-condition-classifier")

# Train and track
with mlflow.start_run():
    mlflow.xgboost.autolog()
    model = XGBClassifier(use_label_encoder=False, eval_metric="logloss")
    model.fit(X_train, y_train, eval_set=[(X_test, y_test)], verbose=False)
```

---

### MLflow: Custom Logging
```python
from sklearn.metrics import accuracy_score

with mlflow.start_run():
    model = XGBClassifier(use_label_encoder=False, eval_metric="logloss")
    model.fit(X_train, y_train)
    y_pred = model.predict(X_test)

    mlflow.log_param("eval_metric", "logloss")
    mlflow.log_metric("accuracy", accuracy_score(y_test, y_pred))
    # mlflow.log_artifact("confusion_matrix.png")
    # mlflow.log_model(model, "xgb-classifier")
```

---

# 🗺️ How the Two Modules Connect

```
Your Data
    │
    ▼
┌─────────────────────────────────────┐
│  MODULE 1: AutoML                   │
│  - AutoML trains MANY models        │
│  - Featurization applied            │
│  - Best model surfaced by metric    │
│  - Great for: fast exploration      │
└─────────────────────────────────────┘
         ─── OR ───
┌─────────────────────────────────────┐
│  MODULE 2: MLflow in Notebooks      │
│  - You write the training code      │
│  - MLflow tracks EACH experiment    │
│  - You control the iteration        │
│  - Great for: custom experimentation│
└─────────────────────────────────────┘
         ─── BOTH LOG TO ───
    Azure Machine Learning Studio
    (Unified experiment history)
```

**Use AutoML** when you want Azure to automatically search the model space. **Use MLflow in notebooks** when you have custom training code and want reproducible, comparable runs.

---

# 📚 Azure Machine Learning — AutoML Study Notes

---

## Page 1: What Is Automated ML (AutoML)?

**Source:** `concept-automated-ml`

### Overview

AutoML automates the time-consuming, iterative tasks of machine learning model development. It allows data scientists, analysts, and developers to build ML models **at scale** with efficiency and productivity while maintaining model quality. It is grounded in Microsoft Research breakthroughs.

### How AutoML Works

1. AutoML creates **many pipelines in parallel**, each trying different algorithms + parameters.
2. It iterates through ML algorithms paired with **feature selections**.
3. Each iteration produces a model with a **training score** against a target metric.
4. The process stops when **exit criteria** are met.
5. The output is a **Python serialized `.pkl` file** containing the model and data preprocessing steps.

**Workflow Steps:**

1. Identify the ML problem (classification, regression, forecasting, computer vision, NLP)
2. Choose code-first (SDK v2 / CLI v2) or no-code (Azure ML Studio)
3. Specify labeled training data source
4. Configure AutoML parameters (iterations, hyperparameters, featurization, metrics)
5. Submit the training job
6. Review results

---

### Task Types

#### 🔵 Classification

- Supervised learning where the model predicts **which category** new data falls into.
- Uses deep neural network text featurizers.
- **Examples:** Fraud detection, handwriting recognition, object detection.
- **Real notebook example:** _Bank Marketing_ — predict if a client will subscribe to a term deposit.

#### 🔵 Regression

- Supervised learning predicting **numerical output values** based on independent predictors.
- Estimates the relationship among independent predictor variables.
- **Examples:** Predicting automobile price based on gas mileage and safety rating.
- **Real notebook example:** _Hardware Performance_ — predict CPU performance.

#### 🔵 Time-Series Forecasting

- Builds predictions about future values over time.
- Treats the problem as a **multivariate regression** — past values are "pivoted" into dimensions.
- Advanced features:
    - Holiday detection and featurization
    - Auto-ARIMA, Prophet, ForecastTCN (DNN learners)
    - Rolling-origin cross validation
    - Configurable lags and rolling window aggregate features
- **Real notebook example:** _Energy Demand_ — predict future electricity consumption.

#### 🔵 Computer Vision

- Generates models trained on **image data**.
- Integrates with Azure ML data labeling.

|Task|Description|
|---|---|
|Multi-class image classification|One label per image (e.g., cat OR dog OR duck)|
|Multi-label image classification|Multiple labels per image (e.g., cat AND dog)|
|Object detection|Locate objects with a **bounding box**|
|Instance segmentation|Identify objects at the **pixel level** using polygons|

#### 🔵 Natural Language Processing (NLP)

- Generates models trained on **text data**.
- Supports: Text classification, named entity recognition (NER).
- Uses the latest **pre-trained BERT models**.
- Multilingual support for **104 languages**.
- Distributed training via **Horovod**.

---

### Training, Validation & Test Data

- AutoML uses **validation data** during training to tune hyperparameters.
- This can introduce **model evaluation bias** (the model keeps fitting to validation data).
- **Test data** (separate from validation) evaluates the **final recommended model** to confirm there is no bias.
- > ⚠️ Test dataset evaluation is a **preview feature** and may change.
    

---

### Feature Engineering & Featurization

- **Featurization** = the combined application of feature engineering, scaling, and normalization.
- AutoML applies featurization **automatically** — but it can be customized.
- Featurization steps become **part of the model** — so when you use the model for predictions, the same steps are applied automatically to input data.
- Customization options: Encoding, transforms, enabled via Studio or SDK.

---

### Ensemble Models

- AutoML supports **ensemble models** (enabled by default).
- Ensemble = combining multiple models rather than relying on one.
- Two methods:
    - **Voting:** Weighted average of predicted class probabilities (classification) or targets (regression).
    - **Stacking:** Combines heterogeneous models; trains a **meta-model** on their outputs.
        - Default meta-model: `LogisticRegression` (classification), `ElasticNet` (regression/forecasting).
- Uses the **Caruana Ensemble Selection Algorithm** to pick which models to include.
    - Initializes with up to 5 best models, verifies they are within **5% threshold** of best score.
    - Adds models iteratively if they improve the ensemble score.

---

### AutoML & ONNX

- AutoML models can be converted to **ONNX format** for cross-platform deployment.
- ONNX enables use in **C# apps** (via ML.NET) without recoding or REST endpoint latency.
- The **ONNX Runtime** also supports C# API inference.

---

---

## Page 2: Data Featurization in AutoML

**Source:** `how-to-configure-auto-features` (SDK v1 reference, concepts apply broadly)

### Feature Engineering vs. Featurization

- **Feature engineering:** Using domain knowledge to create features that help ML algorithms learn better.
- **Featurization:** The umbrella term in AutoML for scaling + normalization + feature engineering applied automatically.

---

### Featurization Configuration Options

|Setting|Behavior|
|---|---|
|`"featurization": 'auto'`|Default — data guardrails + featurization applied automatically|
|`"featurization": 'off'`|No automatic featurization steps|
|`"featurization": FeaturizationConfig`|Custom featurization steps specified|

---

### Automatic Featurization Steps

|Step|Description|
|---|---|
|Drop high cardinality / no variance features|Removes columns that are all-missing, all-same, or are IDs/hashes/GUIDs|
|Impute missing values|Numeric → column mean; Categorical → most frequent value|
|Generate more features|DateTime → year, month, day, hour, etc.; Text → unigrams, bigrams, trigrams|
|Transform & encode|Numeric with few unique values → categorical; Low cardinality → one-hot encoding; High cardinality → one-hot-hash encoding|
|Word embeddings|Text tokens → sentence vectors via pretrained model|
|Cluster distance|k-means clustering; produces distance-to-centroid features|

> ⚠️ Only steps marked with `*` (impute missing values, impute with mean/most frequent, generate more features) are supported in **ONNX format exports**.

---

### Scaling & Normalization Techniques

|Technique|Description|
|---|---|
|StandardScaler|Removes mean, scales to unit variance|
|MinMaxScaler|Scales each feature between its column min and max|
|MaxAbsScaler|Scales by maximum absolute value|
|RobustScaler|Scales by quantile range (robust to outliers)|
|PCA|Linear dimensionality reduction via Singular Value Decomposition|
|TruncatedSVD|Linear dimensionality reduction — works on sparse matrices without centering|
|Normalizer|Rescales each sample so its L1 or L2 norm equals 1|

**Example:** A run using `LogisticRegression` might show `RobustScaler` in the pipeline steps:

```python
[('RobustScaler', RobustScaler(quantile_range=[10, 90])),
 ('LogisticRegression', LogisticRegression(C=0.184, class_weight='balanced'))]
```

---

### Data Guardrails

Data guardrails **detect and correct potential data problems** before training.

**Three states:**

|State|Meaning|
|---|---|
|Passed|No problems detected|
|Done|Changes were applied — review them|
|Alerted|Problem detected but couldn't be fixed — user action required|

**Supported Guardrails:**

|Guardrail|What it checks|
|---|---|
|Missing feature values imputation|Detects and imputes missing values|
|High cardinality feature detection|Detects and handles ID-like features|
|Validation split handling|Auto-splits data if < 20,000 rows (uses cross-validation)|
|Class balancing detection|Warns if classes are imbalanced|
|Memory issues detection|Detects if forecasting config may cause OOM|
|Frequency detection|Ensures time series data points align with detected frequency|
|Cross validation|Applied for datasets < 20,000 samples|
|Short series handling|Drops or pads series with insufficient data points|

---

### Customizing Featurization

```python
featurization_config = FeaturizationConfig()
featurization_config.blocked_transformers = ['LabelEncoder']
featurization_config.drop_columns = ['aspiration', 'stroke']
featurization_config.add_column_purpose('engine-size', 'Numeric')
featurization_config.add_column_purpose('body-style', 'CategoricalHash')
featurization_config.add_transformer_params('Imputer', ['engine-size'], {"strategy": "median"})
featurization_config.add_transformer_params('HashOneHotEncoder', [], {"number_of_bits": 3})
```

Supported customizations:

|Customization|Definition|
|---|---|
|Column purpose update|Override the auto-detected feature type for a column|
|Transformer parameter update|Update parameters for a transformer (e.g., Imputer strategy)|
|Block transformers|Prevent specific transformers from being used|

---

### BERT Integration in AutoML

- **BERT** (Bidirectional Encoder Representations from Transformers) is used in the AutoML featurization layer for text and other columns.
- To enable: Set `enable_dnn: True` and use a **GPU compute** (min `STANDARD_NC6`).
- AutoML compares BERT against a bag-of-words baseline; if BERT wins, it's used for full featurization.
- Supports ~100 languages; selects the appropriate BERT model:
    - English → English BERT
    - German → German BERT
    - Others → Multilingual BERT

**Example — triggering German BERT:**

```python
featurization_config = FeaturizationConfig(dataset_language='deu')
automl_settings = {
    "featurization": featurization_config,
    "enable_dnn": True
}
```

---

### Featurization Transparency

You can inspect what featurization was applied:

```python
# Get engineered feature names
fitted_model.named_steps['timeseriestransformer'].get_engineered_feature_names()
# Output: ['A', 'B', 'A_WASNULL', 'B_WASNULL', 'year', 'half', 'quarter', 'month', ...]

# Get featurization summary
fitted_model.named_steps['timeseriestransformer'].get_featurization_summary()
# Output: [{'RawFeatureName': 'A', 'TypeDetected': 'Numeric', 'Dropped': 'No',
#           'EngineeredFeatureCount': 2, 'Transformations': ['MeanImputer', 'ImputationMarker']}, ...]
```

---

---

## Page 3: Forecasting Methods in AutoML

**Source:** `concept-automl-forecasting-methods`

### Two Categories of Forecasting Models

#### 1. Time Series Models

- Use **historical values of the target** to predict future values.
- Formula: `y(t+1) = f(y_t, y_t-1, ..., y_t-s)`
- Where `s` = amount of history used (also a parameter).
- **Recursive** — forecasts are generated one period at a time; previous forecasts feed back into the model.
- **Risk:** Recursive models **compound prediction error** over many future periods.

#### 2. Regression (Explanatory) Models

- Use **predictor variables** to forecast target values.
- Formula: `y = g(price, day_of_week, holiday)`
- **Direct forecasters** — generate all forecasts up to the horizon in **one attempt** (no compounding error).
- **Limitation:** Predictor variables must be **known in advance** up to the forecast horizon.
- Can be augmented with **lagged quantities** (historical values of target/predictors) → creates a hybrid model.

**Example:** Forecasting orange juice demand:

- Time series: `demand(t+1) = f(demand_t, demand_t-1, ...)`
- Regression: `demand = g(price, day_of_week, is_holiday)` — but price must be known in the future!

---

### Supported Forecasting Models

|Category|Models|
|---|---|
|Time series models|Naive, Seasonal Naive, Average, Seasonal Average, ARIMA(X), Exponential Smoothing|
|Regression models|Linear SGD, LARS LASSO, Elastic Net, Prophet, K Nearest Neighbors, Decision Tree, Random Forest, Extremely Randomized Trees, Gradient Boosted Trees, LightGBM, XGBoost, **TCNForecaster**|
|Ensemble|Soft voting ensemble using Caruana Ensemble Selection Algorithm|

> ⚠️ **TCNForecaster** (deep neural network) **cannot** be included in ensembles. Stack ensemble is **disabled by default** for forecasting to prevent overfitting.

---

### Model Grouping

When a dataset has **multiple time series**, AutoML uses:

- **1:1 (each series in a separate group):** Naive, Seasonal Naive, Average, Seasonal Average, ARIMA, ARIMAX, Prophet, Exponential Smoothing.
- **N:1 (all series in one group):** LightGBM, XGBoost, Random Forest, etc.

**Example:** A retail dataset with products JUICE1 and BREAD3 → ARIMA trains one model per SKU, LightGBM trains one model for all SKUs.

---

### Data Format: "Wide" Tabular

**Simple format:**

|timestamp|quantity|
|---|---|
|2012-01-01|100|
|2012-01-02|97|

**Complex format (multiple series):**

|timestamp|SKU|price|advertised|quantity|
|---|---|---|---|---|
|2012-01-01|JUICE1|3.5|0|100|
|2012-01-01|BREAD3|5.76|0|47|

The `SKU` column is a **time series ID column** — grouping by it produces individual series.

---

### Missing Data Handling

AutoML handles two types of missing data:

1. **Missing cell values** — a value is absent in the table.
2. **Missing rows** — an entire observation row is absent for an expected time point.

**Default imputation:**

|Column type|Default method|
|---|---|
|Target|Forward fill (last observation carried forward)|
|Numeric feature|Median value|
|Categorical feature|Implicit — missing becomes its own category during encoding|

---

### Automated Feature Engineering (Forecasting)

**Default engineered features:**

- Calendar features from the time index (day of week, month, quarter, etc.)
- Categorical features from time series IDs
- Numeric encoding of categorical types

**Optional engineered features:**

- Holiday indicator features (region-specific)
- **Lags** of target quantity
- **Lags** of feature columns
- **Rolling window aggregations** (e.g., rolling 7-day average of target)
- **STL decomposition** (Seasonal and Trend Decomposition using Loess)

---

### Nonstationary Time Series

- **Nonstationary:** A series where mean and/or variance **change over time** (e.g., an upward-trending series).
- AutoML automatically detects nonstationarity and applies a **differencing transform** to mitigate it.
- **First differences:** `Δy_t = y_t - y_t-1` — converts a trending series into a roughly stationary one.

---

### Data Length Requirements

Minimum training observations (with user-provided validation data):

> `T = H + max(l_max, s_window) + 1` Where: H = forecast horizon, l_max = max lag order, s_window = rolling window size

With cross-validation:

> `T = 2H + (n_CV - 1) × n_step + max(l_max, s_window) + 1`

---

---

## Page 4: AutoML for Computer Vision

**Source:** `how-to-auto-train-image-models`

### Task Types & SDK Syntax

|Task|SDK v2 syntax|
|---|---|
|Multi-class image classification|`image_classification()`|
|Multi-label image classification|`image_classification_multilabel()`|
|Object detection|`image_object_detection()`|
|Instance segmentation|`image_instance_segmentation()`|

### Data Format: JSONL

Training data must be provided as **labeled images** in **JSONL (JSON Lines)** format, then wrapped in an **MLTable**.

**Classification JSONL example:**

```json
{"image_url": "azureml://.../Image_01.png",
 "image_details": {"format": "png", "width": "2230px", "height": "4356px"},
 "label": "cat"}
```

**Object detection JSONL example:**

```json
{"image_url": "azureml://.../Image_01.png",
 "label": {"label": "cat", "topX": "1", "topY": "0", "bottomX": "0", "bottomY": "1", "isCrowd": "true"}}
```

> ⚠️ Minimum 10 images to submit a job; **10–15 samples per label** recommended as a minimum for a usable model.

---

### Compute Requirements

- **GPU required** — NC or ND families.
- Multi-GPU VMs speed up training via parallelism.
- Multi-node compute enables faster hyperparameter tuning via parallelism.
- Recommended for best performance: `STANDARD_NC24r` or `STANDARD_NC24rs_V3` (RDMA capable).

---

### Primary Metrics

|Task|Primary metric|
|---|---|
|Image classification|Accuracy|
|Image classification multi-label|Intersection over Union (IoU)|
|Object detection|Mean Average Precision (mAP)|
|Instance segmentation|Mean Average Precision (mAP)|

---

### Experiment Modes

#### 1. Automatic Sweeps (AutoMode) — recommended starting point

- Set `max_trials > 1` and **do not** define a search space.
- AutoML automatically determines which hyperparameter region to explore.
- Running **10–20 trials** works well on many datasets.

#### 2. Individual Trials

- Directly control the model architecture via `model_name` parameter.
- Uses default hyperparameters for the chosen architecture.

#### 3. Manual Sweeps

- Define a search space of model architectures and hyperparameters.
- Specify a sampling method and early termination policy.

---

### Supported Model Architectures

**Legacy models:**

|Task|Architectures|
|---|---|
|Image classification|MobileNetV2, ResNet (18/34/50/101/152), ResNeSt, SE-ResNeXt50, ViT (small/base/large)|
|Object detection|YOLOv5*, Faster RCNN ResNet FPN, RetinaNet ResNet FPN|
|Instance segmentation|MaskRCNN ResNet FPN|

**HuggingFace / MMDetection models (new backend):**

|Task|Examples|
|---|---|
|Classification|BEiT (`microsoft/beit-base-patch16-224`), ViT (`google/vit-base-patch16-224`), DeiT, SwinV2|
|Object detection|Sparse R-CNN, Deformable DETR, VFNet, YOLOF|
|Instance segmentation|Mask R-CNN (`mmd-3x-mask-rcnn_swin-t-p4-w7_fpn_1x_coco`)|

---

### Hyperparameter Sweep Sampling Methods

|Method|Syntax|
|---|---|
|Random|`random`|
|Grid|`grid`|
|Bayesian|`bayesian`|

> Only `random` and `grid` support **conditional hyperparameter spaces**.

---

### Early Termination Policies

|Policy|Purpose|
|---|---|
|Bandit policy|Terminates runs that fall outside a slack factor from the best run|
|Median stopping policy|Terminates runs performing below the median of completed runs|
|Truncation selection policy|Cancels a percentage of the lowest-performing runs at each interval|

---

### Data Augmentation

AutoML automatically applies augmentation — no hyperparameter controls it.

|Task|Training augmentation|
|---|---|
|Image classification|Random resize/crop, horizontal flip, color jitter, normalization|
|Object detection / segmentation|Random crop around bounding boxes, expand, horizontal flip, normalization|
|YOLOv5|Mosaic, random affine (rotation, translation, scale, shear), horizontal flip|

**Disable augmentations (object detection / segmentation only):**

```yaml
training_parameters:
  advanced_settings: >
    {"apply_mosaic_for_yolo": false}
    {"apply_automl_train_augmentations": false}
```

---

### Incremental Training

- After a job finishes, you can further train using the **checkpoint** from a prior job.
- Pass the `checkpoint_run_id` parameter to continue training from that point.

---

---

## Page 5: AutoML for NLP

**Source:** `how-to-auto-train-nlp-models`

### Supported NLP Task Types

|Task|SDK v2 syntax|Description|
|---|---|---|
|Multiclass text classification|`text_classification()`|One class per sample (e.g., "Comedy")|
|Multilabel text classification|`text_classification_multilabel()`|Multiple classes per sample (e.g., "Comedy, Romance")|
|Named Entity Recognition (NER)|`text_ner()`|Tag tokens in sequences (e.g., extract person names, locations)|

---

### Data Formats

**Multiclass / Multilabel — CSV format:**

```
text,labels
"I love watching Chicago Bulls games.","NBA"
"Tom Brady is a great player.","NFL"
```

**Multilabel — multiple labels:**

```
"The four most popular leagues are NFL, MLB, NBA and NHL","football,baseball,basketball,hockey"
```

**NER — CoNLL format (two columns, space-separated):**

```
Hudson   B-loc
Square   I-loc
is       O
a        O
famous   O
place    O
```

---

### Data Validation Rules

|Task|Requirement|
|---|---|
|All tasks|At least **50 training samples**|
|Multiclass & Multilabel|Same columns, same order, same data types in train and validation sets; at least **2 unique labels**|
|NER|No empty lines at start; format `{token} {label}` with single space; labels must start with `I-`, `B-`, or `O`; exactly one empty line between samples|

---

### BERT Language Models Used

|Task|Language|Model|
|---|---|---|
|All NLP tasks|English (`eng`)|English BERT (cased or uncased)|
|All NLP tasks|German (`deu`)|German BERT|
|All NLP tasks|Other (104 languages)|Multilingual BERT|

**Example — setting dataset language:**

```python
text_classification_job.set_featurization(dataset_language='eng')
```

---

### Long-Range Text

- If >10% of samples contain **more than 128 tokens**, enable the long-range text feature.
- Requires `NC6` or higher GPU (NCv3 or NDv2 series).

---

### Supported Pretrained NLP Model Algorithms

|Model family|Examples|
|---|---|
|BERT|`bert-base-cased`, `bert-large-uncased`, `bert-base-multilingual-cased`|
|DistilBERT|`distilbert-base-cased`, `distilbert-base-uncased`|
|RoBERTa|`roberta-base`, `roberta-large`, `distilroberta-base`|
|XLM-RoBERTa|`xlm-roberta-base`, `xlm-roberta-large`|
|XLNet|`xlnet-base-cased`, `xlnet-large-cased`|
|HuggingFace (preview)|Any model from HuggingFace Hub (e.g., `microsoft/deberta-large-mnli`)|

> Large models (`bert-large`, `roberta-large`, etc.) are more performant but require **more GPU memory and time**. NDv2-series VMs recommended.

---

### Hyperparameters for NLP Sweeping

|Parameter|Description|
|---|---|
|`gradient_accumulation_steps`|Number of backward passes before optimizer step — effectively increases batch size|
|`learning_rate`|Initial learning rate (float in [0, 1])|
|`learning_rate_scheduler`|Type of LR scheduler (`linear`, `cosine`, `cosine_with_restarts`, `polynomial`, `constant`)|
|`model_name`|Which pretrained model to use|
|`number_of_epochs`|Training epochs|
|`training_batch_size`|Batch size during training|
|`warmup_ratio`|Fraction of training steps used for LR warmup|
|`weight_decay`|L2 regularization parameter|

**Example sweep config (YAML):**

```yaml
limits:
  timeout_minutes: 120
  max_trials: 4
  max_concurrent_trials: 2
sweep:
  sampling_algorithm: grid
  early_termination:
    type: bandit
    evaluation_interval: 10
    slack_factor: 0.2
search_space:
  - model_name:
      type: choice
      values: [bert_base_cased, roberta_base]
    number_of_epochs:
      type: choice
      values: [3, 4]
```

---

### Distributed Training

AutoML NLP supports **distributed training** on multi-GPU clusters:

```python
max_concurrent_iterations = number_of_vms
enable_distributed_dnn_training = True
```

- Scheduled in **powers of two** (e.g., 1, 2, 4, 8 VMs).
- Maximum: **32 VMs**.
- Unlike other tasks, NLP only supports **hold-out validation** (not cross-validation).

---

---

## 📖 Master Vocabulary Section

|Term|Definition|Example|
|---|---|---|
|**AutoML**|Automated Machine Learning — automatically trains and tunes ML models|Azure ML running 50 pipeline iterations to find the best fraud detection model|
|**Pipeline**|A sequence of data processing and model training steps chained together|`RobustScaler → LogisticRegression`|
|**Exit criteria**|Conditions that stop the AutoML training loop|Time limit reached or target accuracy achieved|
|**Featurization**|The combined process of feature engineering + scaling + normalization|Converting a DateTime column into year, month, day, hour features|
|**Feature engineering**|Creating new informative features from raw data using domain knowledge|Extracting "day of week" from a timestamp column|
|**Imputation**|Filling in missing values with statistical estimates|Replacing missing age values with the column mean|
|**High cardinality**|A feature with many unique values (like IDs or hashes)|A user_id column with millions of unique values — typically dropped|
|**One-hot encoding**|Converting a categorical variable into binary columns, one per category|`color: [red, blue, green]` → `color_red, color_blue, color_green`|
|**Ensemble model**|A model combining predictions from multiple individual models|Voting across LogisticRegression, LightGBM, and RandomForest predictions|
|**Voting ensemble**|Ensemble method using weighted average of predicted probabilities|Final prediction = 0.4 × Model_A + 0.35 × Model_B + 0.25 × Model_C|
|**Stacking ensemble**|Combines heterogeneous models using a meta-model trained on their outputs|Training ElasticNet on the outputs of LightGBM, RandomForest, and XGBoost|
|**Meta-model**|The second-level model in stacking that learns from base model outputs|ElasticNet (for regression) or LogisticRegression (for classification)|
|**ONNX**|Open Neural Network Exchange — cross-platform model format|Exporting an Azure ML model to run in a C# application via ML.NET|
|**Supervised learning**|Training a model on labeled input-output pairs|Training a model on images with "cat" and "dog" labels|
|**Classification**|Predicting a categorical label for input data|Spam vs. not spam email classification|
|**Regression**|Predicting a continuous numerical value|Predicting house price in dollars|
|**Time series**|A sequence of data points indexed in time order|Daily sales numbers recorded over 3 years|
|**Forecast horizon**|How far into the future predictions are made|Forecasting the next 30 days of sales|
|**Lag feature**|A historical value of a variable used as a predictor|Using demand from 2 days ago to predict today's demand|
|**Nonstationary**|A time series where mean/variance change over time|A trending upward sales series|
|**Differencing**|Subtracting consecutive observations to remove trends|`Δy_t = y_t - y_t-1` converts a trending series to stationary|
|**ARIMA**|Autoregressive Integrated Moving Average — a classical time series model|Forecasting monthly airline passenger counts|
|**Rolling window**|An aggregation over a sliding time window|7-day rolling average of website traffic|
|**STL decomposition**|Seasonal and Trend decomposition using Loess — separates trend, season, residual|Decomposing monthly retail sales into trend + holiday seasonality + noise|
|**Cross-validation**|Evaluating a model by training/testing across multiple data splits|10-fold CV for datasets with < 1,000 samples|
|**Data guardrail**|An automated check that detects and corrects data quality issues|AutoML detecting that class A has 95% of samples vs. class B's 5%|
|**Bounding box**|A rectangle around a detected object in an image|A box drawn around a detected car in a photo|
|**Instance segmentation**|Identifying objects at pixel level using polygons|Pixel-by-pixel outline of each car in a traffic image|
|**MLTable**|Azure ML's format for referencing structured training data|A YAML file pointing to image JSONL files for computer vision tasks|
|**JSONL**|JSON Lines — one JSON object per line, used for CV training data|Each line represents one labeled image with its URL and label|
|**BERT**|Bidirectional Encoder Representations from Transformers — pretrained NLP model|Encoding "The cat sat on the mat" into feature vectors|
|**NER**|Named Entity Recognition — tagging tokens with semantic labels|Tagging "New York" as B-loc, I-loc in text|
|**CoNLL format**|Two-column space-separated format for NER training data|`Hudson B-loc` / `Square I-loc`|
|**Hyperparameter**|A configuration value set before training that controls the learning process|Learning rate, number of epochs, batch size|
|**Hyperparameter sweep**|Searching over a range of hyperparameter values to find the best combination|Trying learning rates 0.0001–0.01 across 20 trials|
|**Bandit policy**|Early termination — stops runs that fall outside a slack factor from the best|Stops a trial if its accuracy is >20% below the best trial's at the same step|
|**Gradient accumulation**|Summing gradients over multiple batches before updating weights|Simulates a larger batch size on GPUs with limited memory|
|**Warmup ratio**|Fraction of training steps during which learning rate gradually increases|First 10% of steps ramp LR from 0 to the target, then decay|
|**Distributed training**|Training a model across multiple GPUs or machines simultaneously|4 VMs each training on a shard of the NLP dataset, using Horovod|
|**Horovod**|A distributed deep learning training framework|Synchronizing gradients across 8 GPU nodes during BERT fine-tuning|
|**MoE / Mosaic (YOLOv5)**|Data augmentation — stitching 4 images into one composite for training|Combining 4 different scene images into one training sample|
|**Data augmentation**|Artificially expanding training data with transformations|Flipping, cropping, color-jittering training images|
|**Incremental training**|Continuing training from a saved model checkpoint|Loading a YOLOv5 checkpoint after 50 epochs and training 20 more|
|**Caruana algorithm**|Ensemble selection algorithm that greedily adds models improving ensemble score|Starts with top 5 models, adds a 6th only if it improves the overall score|
|**PCA**|Principal Component Analysis — dimensionality reduction technique|Reducing 500 numeric features to 50 principal components|
|**StandardScaler**|Normalizes features to zero mean and unit variance|Height values scaled from [150cm, 200cm] to roughly [-1.5, 1.5]|
|**Predict_proba**|A model method returning class probability estimates|Returns `[0.1, 0.85, 0.05]` meaning 85% probability of class B|
|**ThresholdIng (NLP)**|Setting the cutoff probability for assigning a positive label in multilabel tasks|Threshold 0.3 → more labels assigned; Threshold 0.7 → fewer, more confident labels|

---

## 🔗 Pages Covered

1. **Main AutoML Concepts** — What is AutoML, task types, ensembles, ONNX
2. **Data Featurization** — Automatic featurization, scaling, data guardrails, BERT integration, transparency
3. **Forecasting Methods** — Time series vs. regression models, model grouping, missing data, feature engineering
4. **Computer Vision AutoML** — Task types, JSONL data format, model architectures, sweeping, augmentation
5. **NLP AutoML** — Text classification, NER, BERT models, CoNLL format, distributed training