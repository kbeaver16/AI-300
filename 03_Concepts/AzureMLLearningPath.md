tags:
  - azure
  - machine-learning
  - automl
  - mlflow
  - python
  - sdk-v2
  - study-notes
  - classification
  - model-evaluation
source: "https://learn.microsoft.com/en-us/training/paths/automate-machine-learning-model-selection-azure-machine-learning/"
date: 2026-06-04
level: Beginner
estimated-time: "1 hr 10 min"
modules: 2
---

# Azure ML: Automate Machine Learning Model Selection

> **Source:** [Microsoft Learn Learning Path](https://learn.microsoft.com/en-us/training/paths/automate-machine-learning-model-selection-azure-machine-learning/)
> **Level:** Beginner | **Duration:** ~1 hr 10 min | **Modules:** 2

This learning path covers two complementary approaches for finding the best ML model in Azure:
1. **AutoML** — automatically tries multiple algorithms and preprocessing steps on your behalf
2. **MLflow** — lets you manually experiment in Jupyter notebooks while systematically tracking all results

---

## Table of Contents

- [[#Module 1 – Find the Best Classification Model with AutoML]]
  - [[#Unit 1 – Introduction]]
  - [[#Unit 2 – Preprocess Data and Configure Featurization]]
  - [[#Unit 3 – Run an AutoML Experiment]]
  - [[#Unit 4 – Evaluate and Compare Models]]
  - [[#Unit 5 – Exercise]]
  - [[#Unit 6 – Knowledge Check]]
  - [[#Unit 7 – Summary]]
- [[#Module 2 – Track Model Training in Jupyter Notebooks with MLflow]]
  - [[#Unit 1 – Introduction (MLflow)]]
  - [[#Unit 2 – Configure MLflow for Model Tracking in Notebooks]]
  - [[#Unit 3 – Train and Track Models in Notebooks]]
  - [[#Unit 4 – Exercise (MLflow)]]
  - [[#Unit 5 – Knowledge Check (MLflow)]]
  - [[#Unit 6 – Summary (MLflow)]]
- [[#Linked Reference – AutoML Configuration Deep Dive]]
  - [[#Supported Algorithms (All Task Types)]]
  - [[#Primary Metrics by Task Type]]
  - [[#Featurization Configuration]]
  - [[#Exit Criteria (set_limits)]]
  - [[#Validation Strategy]]
  - [[#Distributed Training]]
  - [[#AutoML in Pipelines]]
- [[#Linked Reference – Evaluate AutoML Experiment Results]]
  - [[#Classification Charts]]
  - [[#Classification Metrics Reference]]
  - [[#Regression and Forecasting Metrics]]
  - [[#Responsible AI Dashboard]]
- [[#Linked Reference – MLTable Data Asset]]
- [[#Vocabulary / Glossary]]
- [[#Examples]]
- [[#Key Takeaways]]

---

## Module 1 – Find the Best Classification Model with AutoML

**Duration:** 37 min | **Units:** 7
**Reference:** [Module page](https://learn.microsoft.com/en-us/training/modules/find-best-classification-model-automated-machine-learning/)

### Learning Objectives

- Prepare data to use AutoML for classification
- Configure and run an AutoML experiment
- Evaluate and compare models

---

### Unit 1 – Introduction

**Automated Machine Learning (AutoML)** automates the trial-and-error process of selecting the best-performing ML model. It iterates through multiple preprocessing transformations and algorithms to find the best combination for your dataset.

**Access methods:**

| Method | Best For |
|---|---|
| Azure ML Studio (no-code UI) | Quick experimentation, business users |
| Azure CLI | Scripting, DevOps pipelines |
| **Python SDK v2** | Data scientists, full programmatic control |

**Supported AutoML task types:**

| Task Type | Description |
|---|---|
| Classification | Predict a discrete class/label |
| Regression | Predict a continuous numeric value |
| Time-series Forecasting | Predict future values from historical sequences |
| Image Classification | Multi-class and multi-label image classification |
| Image Object Detection | Detect and locate objects in images |
| Image Instance Segmentation | Pixel-level object identification |
| NLP Text Classification | Multi-class and multi-label text classification |
| NLP Named Entity Recognition (NER) | Identify entities in text |

> [!NOTE]
> This module focuses on **tabular classification** using the **Python SDK v2**.

---

### Unit 2 – Preprocess Data and Configure Featurization

**Reference:** [How to create a MLTable data asset](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-mltable)

#### Data Requirements

Input data **must** be a registered **MLTable data asset** — not a raw CSV or Parquet file passed directly.

- MLTable is a YAML-based blueprint defining how to load data files into memory as a Pandas or Spark DataFrame
- Specifies: storage location, file format, delimiters, headers, column types, and data subsets
- The MLTable file and data files live together in the same folder

```python
from azure.ai.ml.constants import AssetTypes
from azure.ai.ml import Input

# Reference a registered MLTable data asset (version 1)
my_training_data_input = Input(
    type=AssetTypes.MLTABLE,
    path="azureml:input-data-automl:1"   # friendly-name:version
)
```

**Creating an MLTable programmatically:**

```python
import mltable

# Build from a local CSV
tbl = mltable.from_delimited_files(paths=[{"file": "./train_data/bank_marketing_train_data.csv"}])
tbl.save("./train_data")   # writes MLTable YAML alongside the CSV
```

**Supported MLTable source formats:**

| Method | Source Type |
|---|---|
| `from_delimited_files()` | CSV, TSV |
| `from_parquet_files()` | Parquet |
| `from_delta_lake()` | Delta Lake |
| `from_json_lines_files()` | JSON Lines |
| `from_paths()` | Generic file paths |

#### Automatic Scaling and Normalization

AutoML **automatically** applies scaling/normalization to numeric features before each trial. You do not configure this — AutoML selects the optimal technique per trial and reveals it in the model name after the experiment completes.

| Scaling Technique | How It Works |
|---|---|
| `MaxAbsScaler` | Divides each feature by its maximum absolute value → range [-1, 1] |
| `StandardScalerWrapper` | Subtracts mean, divides by std dev → zero mean, unit variance |
| `MinMaxScaler` | Scales to fixed range [0, 1] |
| `RobustScaler` | Uses median and IQR — robust to outliers |

> [!NOTE]
> Featurization steps become part of the underlying model. When deployed, the same transformations applied during training are **automatically applied to new input data** at inference time.

#### Featurization

**Featurization** = automatic feature engineering AutoML applies to your data before training each model.

- **Enabled by default** (`mode: 'auto'`)
- Can be set to `'off'` (disabled) or `'custom'` (user-defined steps)
- After the experiment, review which featurization steps were applied in Azure ML Studio

| Featurization Step | What It Does |
|---|---|
| Missing value imputation | Fills `null` values using mean, median, or mode |
| Categorical encoding | Converts text/category columns to numeric representations |
| Drop high-cardinality features | Removes columns with too many unique values (e.g., ID columns, free text) |
| Feature engineering | Derives new features (e.g., extracts year, month, day from a `DateTime` column) |

**Featurization configuration options:**

```python
# Default: auto featurization
classification_job = automl.classification(
    ...,
    # featurization is 'auto' by default
)

# Disable featurization entirely
from azure.ai.ml.automl import featurization_settings
classification_job.set_featurization(mode="off")

# Custom featurization
from azure.ai.ml.automl import ColumnTransformer
classification_job.set_featurization(
    mode="custom",
    transformer_params={
        "Imputer": [ColumnTransformer(fields=["age"], parameters={"strategy": "most_frequent"})]
    }
)
```

---

### Unit 3 – Run an AutoML Experiment

**Reference:** [Set up AutoML training (Python SDK v2)](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-configure-auto-train)

#### Step 1 – Define the Classification Task

```python
from azure.ai.ml import automl

classification_job = automl.classification(
    compute="aml-cluster",                   # name of your compute target
    experiment_name="auto-ml-class-dev",     # groups runs in AML Studio
    training_data=my_training_data_input,    # MLTable Input reference
    target_column_name="Diabetic",           # column to predict
    primary_metric="accuracy",               # metric to optimize
    n_cross_validations=5,                   # k-fold cross-validation folds
    enable_model_explainability=True         # generate feature importance after job
)
```

#### Step 2 – Set Job Limits (`set_limits()`)

```python
classification_job.set_limits(
    timeout_minutes=60,             # total experiment wall-clock timeout
    trial_timeout_minutes=20,       # max time per individual model trial
    max_trials=5,                   # max number of model trials to attempt
    max_concurrent_trials=4,        # parallel trials (must match cluster node count)
    enable_early_termination=True   # stop experiment if no improvement observed
)
```

> [!TIP]
> Match `max_concurrent_trials` to the number of nodes in your compute cluster for maximum efficiency. Each cluster node runs one trial at a time.

#### Step 3 – Configure Training Options

```python
# Block specific algorithms you don't want AutoML to try
classification_job.set_training(
    blocked_training_algorithms=["NaiveBayes", "LinearSVM"],
    enable_vote_ensemble=True,       # allow VotingEnsemble (default: True)
    enable_stack_ensemble=True       # allow StackEnsemble (default: True)
)
```

#### Step 4 – Submit and Monitor

```python
from azure.ai.ml import MLClient
from azure.identity import DefaultAzureCredential

ml_client = MLClient(
    DefaultAzureCredential(),
    subscription_id="<sub-id>",
    resource_group_name="<rg>",
    workspace_name="<workspace>"
)

# Submit the job
returned_job = ml_client.jobs.create_or_update(classification_job)

# Print direct link to monitor in Azure ML Studio
print("Monitor at:", returned_job.studio_url)
```

> [!WARNING]
> If you run the same experiment configuration multiple times, results may vary slightly due to inherent randomness in the algorithms (e.g., random seeds in ensemble selection).

---

### Unit 4 – Evaluate and Compare Models

#### Reviewing Results in Azure ML Studio

Navigate to the completed job in AML Studio. Key tabs:

| Tab | What It Shows |
|---|---|
| **Overview** | Input data asset, experiment settings, best model summary |
| **Models** | All trained models sorted by primary metric (best = top) |
| **Data guardrails** | Results of automatic data quality checks |
| **Metrics** | Charts and metric values for the selected model |

#### Data Guardrails

Before training begins, AutoML automatically runs **data guardrail checks** on your dataset for classification:

| Guardrail | What It Detects |
|---|---|
| Class balancing detection | Severely imbalanced class distribution |
| Missing feature values imputation | `null` / `NaN` values in feature columns |
| High cardinality feature detection | Columns with too many unique values |

**Guardrail status meanings:**

| Status | Meaning | Action Required? |
|---|---|---|
| ✅ **Passed** | No issue detected; no changes made | No |
| 🔵 **Done** | Issue detected and automatically fixed — check what changed | Review changes |
| ⚠️ **Alerted** | Issue detected but **not auto-fixed** | Yes — fix before trusting model |

#### Reading Algorithm Names in the Models Tab

Each model name = **scaling technique + algorithm name**, e.g.:

```
MaxAbsScaler LightGBM
│             └── Algorithm trained
└── Scaling applied to features before training

StandardScalerWrapper XGBoost
RobustScaler RandomForest
MinMaxScaler LogisticRegression
```

#### Model Comparison and Interpretability

- Click **Edit columns** to add/remove metric columns for side-by-side comparison
- Click **Generate model explanations** on the best model to produce a **feature importance** chart
- Feature importance = estimated relative contribution of each input column to the predictions

---

### Unit 5 – Exercise

Hands-on lab: Run an AutoML classification job using the Python SDK v2 in an Azure ML compute instance notebook. Uses a diabetes dataset to predict the `Diabetic` column.

**Key lab steps:**
1. Connect to Azure ML workspace with `MLClient`
2. Register training data as an MLTable asset
3. Configure AutoML classification job
4. Set experiment limits
5. Submit job and retrieve `studio_url`
6. Review Data Guardrails and Models tab in AML Studio
7. Generate model explanations for best model

---

### Unit 6 – Knowledge Check

Key questions covered:
- What data format is required for AutoML input? → **MLTable**
- What does a "Done" guardrail status mean? → Issue detected and auto-fixed; review the change
- How do you limit the total experiment run time? → `set_limits(timeout_minutes=...)`
- What does the model name `MaxAbsScaler LightGBM` tell you? → MaxAbsScaler was used to scale the data before LightGBM was trained

---

### Unit 7 – Summary

AutoML in Azure ML automates the process of:
1. **Preprocessing** data (scaling, normalization, featurization)
2. **Iterating** through algorithm + hyperparameter combinations (trials)
3. **Evaluating** each model against a primary metric
4. **Surfacing** the best model with interpretability support

Use the Python SDK v2 (`automl.classification()`) to configure and submit jobs programmatically, then review results in Azure ML Studio.

---

## Module 2 – Track Model Training in Jupyter Notebooks with MLflow

**Duration:** 33 min | **Units:** 6
**Reference:** [Module page](https://learn.microsoft.com/en-us/training/modules/track-model-training-jupyter-notebooks-mlflow/)

### Learning Objectives 2

- Configure MLflow to use in notebooks
- Use MLflow for model tracking in notebooks

---

### Unit 1 – Introduction (MLflow)

**Scenario:** A data scientist is building a breast cancer detection app for a research lab. Researchers upload tissue images and the app predicts cancer presence. The data scientist uses Jupyter notebooks to iteratively develop and periodically retrain the model. The challenge: tracking all different model versions, parameters, and metrics across dozens of experiments without losing track of what worked.

**Solution:** Use **MLflow** to systematically log and compare all training runs inside Azure ML.

---

### Unit 2 – Configure MLflow for Model Tracking in Notebooks

#### What is MLflow?

> **MLflow** is an open-source platform for managing the end-to-end ML lifecycle — tracking experiments, packaging models, and deploying to production. Azure Machine Learning is natively integrated with MLflow.

**Core MLflow components:**

| Component | Purpose |
|---|---|
| **MLflow Tracking** | Logs parameters, metrics, tags, and artifacts for each run |
| MLflow Models | Standard format for packaging ML models |
| MLflow Model Registry | Centralized model versioning and lifecycle management |
| MLflow Projects | Reproducible ML project packaging |

> [!NOTE]
> This module focuses on **MLflow Tracking** — the component used to log training runs.

#### Required Packages

| Package | Purpose |
|---|---|
| `mlflow` | Open-source core MLflow library (tracking, models, etc.) |
| `azureml-mlflow` | Microsoft plugin — authenticates and routes logs to your AML workspace |

```bash
pip install mlflow azureml-mlflow
```

#### Setup Option 1: Azure ML Compute Instance (Notebooks in Studio)

- MLflow is **pre-configured automatically** when running notebooks on a compute instance
- Just verify both packages are installed — no tracking URI configuration needed
- Azure ML automatically routes all logged data to the correct workspace experiment

```python
# Verify packages are installed (run in notebook cell)
import mlflow
import azureml.mlflow
print(f"MLflow version: {mlflow.__version__}")
```

#### Setup Option 2: Local Jupyter Notebook (your own machine)

**Step 1** — Get the MLflow Tracking URI from Azure ML Studio:
1. Open **Azure ML Studio** → top-right corner
2. Click **View all properties in Azure portal**
3. Copy the **MLflow tracking URI** (format: `azureml://...`)

**Step 2** — Configure in your notebook:

```python
import mlflow

# Point MLflow at your Azure ML workspace
mlflow.set_tracking_uri("azureml://YOUR-REGION.api.azureml.ms/mlflow/v1.0/subscriptions/<sub-id>/resourceGroups/<rg>/providers/Microsoft.MachineLearningServices/workspaces/<workspace>")
```

---

### Unit 3 – Train and Track Models in Notebooks

#### Step 1 – Create / Activate an Experiment

Experiments **group related runs** together (e.g., all training attempts for the cancer detection model):

```python
import mlflow

mlflow.set_experiment("cancer-detection-experiment")
# If omitted, all runs go to an experiment named "Default"
```

#### Step 2 – Autologging (Simplest Method)

**Autologging** automatically captures all relevant information for supported frameworks — no manual log calls needed.

```python
import mlflow
from xgboost import XGBClassifier

mlflow.set_experiment("cancer-detection-experiment")
mlflow.xgboost.autolog()   # enable autologging for XGBoost

with mlflow.start_run():
    model = XGBClassifier(n_estimators=100, max_depth=5, learning_rate=0.1)
    model.fit(X_train, y_train)
    # Automatically logged:
    #   Parameters: n_estimators, max_depth, learning_rate, etc.
    #   Metrics: accuracy, AUC, F1, etc.
    #   Artifacts: model file, feature importance plot
```

**Supported autologging frameworks:**

| Framework | Autolog Call | What Gets Logged |
|---|---|---|
| scikit-learn | `mlflow.sklearn.autolog()` | Params, metrics, model pickle |
| XGBoost | `mlflow.xgboost.autolog()` | Params, metrics, feature importance, model |
| LightGBM | `mlflow.lightgbm.autolog()` | Params, metrics, feature importance, model |
| TensorFlow/Keras | `mlflow.tensorflow.autolog()` | Params, metrics per epoch, model SavedModel |
| PyTorch Lightning | `mlflow.pytorch.autolog()` | Params, metrics per epoch, model checkpoint |
| Spark MLlib | `mlflow.spark.autolog()` | Params, metrics, model |

#### Step 3 – Custom Logging (Full Control)

Use custom logging when you need precise control over what gets tracked, or to log values that autologging doesn't capture:

```python
import mlflow
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score, roc_auc_score
import matplotlib.pyplot as plt

mlflow.set_experiment("diabetes-experiment")

with mlflow.start_run(run_name="logreg-C0.1"):

    # --- Train model ---
    reg_rate = 0.1
    model = LogisticRegression(C=reg_rate, solver="liblinear", max_iter=200)
    model.fit(X_train, y_train)

    # --- Log input parameters (hyperparameters) ---
    mlflow.log_param("regularization_C", reg_rate)
    mlflow.log_param("solver", "liblinear")
    mlflow.log_param("max_iter", 200)

    # --- Log output metrics (must be numeric) ---
    y_pred = model.predict(X_test)
    acc = accuracy_score(y_test, y_pred)
    auc = roc_auc_score(y_test, model.predict_proba(X_test)[:, 1])
    mlflow.log_metric("test_accuracy", acc)
    mlflow.log_metric("AUC", auc)

    # --- Log an artifact (must save file first, then log path) ---
    fig, ax = plt.subplots()
    ax.bar(["Accuracy", "AUC"], [acc, auc])
    ax.set_title("Model Performance")
    fig.savefig("metrics_bar.png")
    mlflow.log_artifact("metrics_bar.png")

    # --- Log the trained model ---
    mlflow.sklearn.log_model(model, artifact_path="logistic-model")
```

#### Custom Logging Functions Reference

| Function | Purpose | Accepted Value Types |
|---|---|---|
| `mlflow.log_param(key, value)` | Log a single input parameter (hyperparameter) | Any (string, int, float, bool) |
| `mlflow.log_params({dict})` | Log multiple parameters at once | Dict of key-value pairs |
| `mlflow.log_metric(key, value)` | Log a single numeric output metric | **Numbers only** |
| `mlflow.log_metrics({dict})` | Log multiple metrics at once | Dict of key-numeric-value pairs |
| `mlflow.log_artifact(local_path)` | Log a file as an artifact | String (local file path) |
| `mlflow.log_artifacts(local_dir)` | Log an entire directory of files | String (local dir path) |
| `mlflow.log_model(model, path)` | Log a trained model in MLflow format | Framework-specific model object |
| `mlflow.set_tag(key, value)` | Attach a string label/tag to the run | Any string value |

#### Combining Autologging + Custom Logging

You can use **both together** — autologging captures standard framework outputs while custom logging adds supplementary data:

```python
mlflow.sklearn.autolog()   # auto-log all standard sklearn metrics

with mlflow.start_run():
    model.fit(X_train, y_train)

    # Add a custom business metric on top of what autologging captured
    false_negatives = ((y_test == 1) & (model.predict(X_test) == 0)).sum()
    mlflow.log_metric("false_negatives", int(false_negatives))
    mlflow.log_metric("estimated_false_negative_cost_usd", false_negatives * 1250.00)
    mlflow.set_tag("model_type", "logistic_regression")
    mlflow.set_tag("dataset_version", "v3")
```

---

### Unit 4 – Exercise (MLflow)

Hands-on lab: Track a diabetes classification model training run in a Jupyter notebook using MLflow.

**Key lab steps:**
1. Install and verify `mlflow` and `azureml-mlflow` on compute instance
2. Set the experiment name with `mlflow.set_experiment()`
3. Enable autologging with `mlflow.sklearn.autolog()`
4. Train a Logistic Regression model inside `with mlflow.start_run():`
5. Add custom metrics via `mlflow.log_metric()`
6. Log a performance plot via `mlflow.log_artifact()`
7. Review logged run in Azure ML Studio → Experiments

---

### Unit 5 – Knowledge Check (MLflow)

Key questions covered:
- Where is the MLflow Tracking URI found for a local notebook setup? → AML Studio → View properties in Azure portal
- What is the difference between `log_param()` and `log_metric()`? → `log_param` = input/hyperparameter (any type); `log_metric` = numeric output metric only
- What does `mlflow.sklearn.autolog()` do? → Automatically logs all sklearn parameters, metrics, and the model without manual log calls
- Where do logged runs appear? → Azure ML Studio → Experiments tab

---

### Unit 6 – Summary (MLflow)

MLflow Tracking lets you:
1. **Group** related training runs under a named experiment
2. **Log** parameters, metrics, plots, and models for each run
3. **Compare** runs side-by-side in Azure ML Studio
4. **Reproduce** any run by checking its logged parameters
5. **Promote** the best run to a registered model for deployment

---

## Linked Reference – AutoML Configuration Deep Dive

**Source:** [Set up AutoML training for tabular data (Python SDK v2)](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-configure-auto-train)

---

### Supported Algorithms (All Task Types)

#### Classification Algorithms

| Algorithm | Notes |
|---|---|
| Logistic Regression | Linear model, fast, good baseline |
| Light GBM | Gradient boosting, often top performer |
| Gradient Boosting | Ensemble of decision trees |
| Decision Tree | Interpretable, prone to overfitting |
| K Nearest Neighbors (KNN) | Distance-based, no training phase |
| Linear SVC | Linear SVM for classification |
| Support Vector Classification (SVC) | Kernel SVM |
| Random Forest | Bagging ensemble of decision trees |
| Extremely Randomized Trees | More randomness than Random Forest |
| XGBoost | Optimized gradient boosting |
| Naive Bayes | Probabilistic, fast on text features |
| Stochastic Gradient Descent (SGD) | Online learning, scalable |

#### Regression Algorithms

| Algorithm | Notes |
|---|---|
| Elastic Net | Lasso + Ridge regularization combined |
| Light GBM | Gradient boosting |
| Gradient Boosting | Ensemble of decision trees |
| Decision Tree | Simple tree regression |
| K Nearest Neighbors | Distance-based |
| LARS Lasso | Least angle regression |
| SGD | Stochastic gradient descent |
| Random Forest | Bagging ensemble |
| Extremely Randomized Trees | High-variance variant of Random Forest |
| XGBoost | Optimized gradient boosting |

#### Time Series Forecasting Algorithms

| Algorithm | Notes |
|---|---|
| AutoARIMA | Auto-tuned ARIMA statistical model |
| Prophet | Facebook's trend + seasonality model |
| Elastic Net | Regression-based forecasting |
| Light GBM | ML-based forecasting |
| Decision Tree | Tree-based forecasting |
| KNN | Distance-based forecasting |
| LARS Lasso | Regularized linear regression |
| Extremely Randomized Trees | Ensemble forecasting |
| Random Forest | Bagging-based forecasting |
| XGBoost | Gradient boosting forecasting |
| TCNForecaster | Temporal Convolutional Network (deep learning) |
| ExponentialSmoothing | Classical exponential smoothing |
| SeasonalNaive | Seasonal last-observation carry-forward |
| Average | Mean of historical values |
| Naive | Last-observation carry-forward |
| SeasonalAverage | Average of same season from prior periods |

> [!TIP]
> Use `allowed_training_algorithms` to whitelist specific algorithms or `blocked_training_algorithms` to exclude them:
> ```python
> classification_job.set_training(
>     allowed_training_algorithms=["LightGBM", "XGBoost", "RandomForest"]
> )
> ```

---

### Primary Metrics by Task Type

#### Classification Primary Metrics

| Metric | Best For | Direction |
|---|---|---|
| `accuracy` | Image classification, sentiment analysis, churn prediction | Maximize |
| `AUC_weighted` | Fraud detection, anomaly/spam detection, imbalanced datasets | Maximize |
| `average_precision_score_weighted` | Sentiment analysis | Maximize |
| `norm_macro_recall` | Churn prediction | Maximize |
| `precision_score_weighted` | High-precision requirements | Maximize |

> [!IMPORTANT]
> For **small datasets**, **highly imbalanced classes**, or when the expected metric value is near 0.0 or 1.0, prefer `AUC_weighted` over `accuracy` as the primary metric.

#### Regression Primary Metrics

| Metric | Best For | Direction |
|---|---|---|
| `r2_score` | Airline delay, salary estimation, bug resolution time | Maximize |
| `normalized_root_mean_squared_error` | Price prediction, inventory optimization | Minimize |
| `normalized_mean_absolute_error` | General regression | Minimize |
| `spearman_correlation` | When rank matters more than exact value | Maximize |

#### Forecasting Primary Metrics

| Metric | Best For |
|---|---|
| `normalized_root_mean_squared_error` | Price prediction, demand forecasting |
| `r2_score` | Inventory optimization |
| `normalized_mean_absolute_error` | General forecasting |

---

### Featurization Configuration

| Mode | Description |
|---|---|
| `'auto'` (default) | Data guardrails + full featurization applied automatically |
| `'off'` | No featurization — raw data passed to algorithms |
| `'custom'` | User-defined transformers applied to specific columns |

---

### Exit Criteria (`set_limits()`)

| Parameter | Description | Default |
|---|---|---|
| `timeout_minutes` | Total experiment wall-clock time limit | 8,640 min (6 days) |
| `trial_timeout_minutes` | Max time for a single model trial | 43,200 min (1 month) |
| `max_trials` | Max number of model trials to attempt | 1,000 |
| `max_concurrent_trials` | Max trials running in parallel | 1 |
| `enable_early_termination` | Stop if no improvement in primary metric | False |

> [!WARNING]
> If `timeout_minutes ≤ 60`, your dataset must be ≤ 10,000,000 cells (rows × columns) or an error occurs.
> Ensemble and model explainability runs happen **after** the timeout window — they are excluded from the clock.

---

### Validation Strategy

AutoML automatically selects the validation approach based on dataset size:

| Training Data Size | Validation Approach |
|---|---|
| **> 20,000 rows** | 10% holdout split (train = 90%, validation = 10%) |
| **1,000 – 20,000 rows** | 3-fold cross-validation |
| **< 1,000 rows** | 10-fold cross-validation |

You can override this with `n_cross_validations` or by providing explicit `validation_data`.

---

### Distributed Training

For very large datasets, AutoML supports distributed training on multi-node clusters:

| Algorithm | Task Types | Max Data Size |
|---|---|---|
| LightGBM | Classification, Regression | ~1 TB |
| TCNForecaster | Forecasting | ~200 GB |

**Configuration:**
```python
classification_job.set_training(
    training_mode="distributed"   # or "non_distributed" (default)
)
classification_job.set_limits(
    max_nodes=4   # number of cluster nodes to use per trial (min 4)
)
```

> [!NOTE]
> Distributed training does **not** support: cross-validation, ensemble models, ONNX export, or code generation. Trials run sequentially (not concurrently) in distributed mode.

---

### AutoML in Pipelines

AutoML jobs can be embedded as steps in Azure ML Pipelines for end-to-end MLOps automation:

```python
from azure.ai.ml.dsl import pipeline
from azure.ai.ml import automl

@pipeline()
def automl_pipeline(training_data, target_column):
    automl_step = automl.classification(
        training_data=training_data,
        target_column_name=target_column,
        primary_metric="accuracy",
        compute="cpu-cluster"
    )
    return {"best_model": automl_step.outputs.best_model}

pipeline_job = automl_pipeline(
    training_data=Input(type=AssetTypes.MLTABLE, path="azureml:diabetes-data:1"),
    target_column="Diabetic"
)
ml_client.jobs.create_or_update(pipeline_job)
```

---

## Linked Reference – Evaluate AutoML Experiment Results

**Source:** [Evaluate AutoML experiment results](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-understand-automated-ml)

---

### Classification Charts

AutoML generates the following evaluation charts for classification experiments:

| Chart | What It Shows |
|---|---|
| **Confusion Matrix** | Actual vs. predicted class counts — systematic errors |
| **ROC Curve** | True positive rate vs. false positive rate at all thresholds |
| **Precision-Recall Curve** | Precision vs. recall tradeoff at all thresholds |
| **Lift Curve** | How much better the model is vs. random prediction |
| **Cumulative Gains Curve** | % of positives captured vs. % of data examined |
| **Calibration Curve** | Model confidence vs. actual proportion correct |

#### Confusion Matrix

- A cell at row `i`, column `j` = number of samples **actually class Cᵢ** that were **predicted as Cⱼ**
- **Diagonal cells** = correct predictions (want these to be large)
- **Off-diagonal cells** = errors (want these to be small/zero)
- View as **Raw** (sample counts) or **Normalized** (% per actual class)
- A good model has most mass along the diagonal

#### ROC Curve (Receiver Operating Characteristic)

- Plots **True Positive Rate (TPR/Recall)** vs. **False Positive Rate (FPR)** at each threshold
- **AUC (Area Under Curve)** = probability that the model ranks a random positive higher than a random negative
- A curve approaching the **top-left corner** = near-perfect model
- A curve along the `y = x` diagonal = random guessing
- Less informative on **highly imbalanced** datasets

#### Precision-Recall Curve

- Plots **Precision** vs. **Recall** at each classification threshold
- Use when the **positive class is rare** or **false negatives are costly**
- A curve approaching the **top-right corner** = best model

#### Cumulative Gains Curve

- Shows what % of all positive samples are captured when examining the top X% of predictions (ranked by confidence)
- Perfect model: captures 100% of positives in the top X% (where X = positive class proportion)
- Baseline: `y = x` (random model)

#### Lift Curve

- Lift = cumulative gain / baseline gain
- Shows how many times better than random the model is at each threshold
- Baseline lift = 1 (y = 1 line)
- Higher lift at small percentages = model confidently identifies positives early

#### Calibration Curve

- Plots model confidence vs. actual proportion of positive samples
- **Well-calibrated model**: follows `y = x` (50% confidence → 50% actually positive)
- **Over-confident model**: S-curve shifted left (claims high confidence too often)
- **Under-confident model**: S-curve shifted right (under-reports confidence)
- Does **not** measure classification accuracy — only confidence reliability

---

### Classification Metrics Reference

All metrics are based on **scikit-learn implementations**.

| Metric | Description | Range | Objective |
|---|---|---|---|
| `accuracy` | % of predictions that exactly match true labels | [0, 1] | Maximize |
| `AUC_weighted` | Weighted avg area under ROC curve | [0, 1] | Maximize |
| `AUC_macro` | Unweighted avg AUC per class | [0, 1] | Maximize |
| `AUC_micro` | AUC computed globally across all classes | [0, 1] | Maximize |
| `average_precision_score_weighted` | Weighted avg precision-recall curve area | [0, 1] | Maximize |
| `balanced_accuracy` | Arithmetic mean of recall per class | [0, 1] | Maximize |
| `f1_score_weighted` | Weighted harmonic mean of precision & recall | [0, 1] | Maximize |
| `f1_score_macro` | Unweighted mean F1 per class | [0, 1] | Maximize |
| `f1_score_micro` | F1 computed globally | [0, 1] | Maximize |
| `log_loss` | Negative log-likelihood (used in logistic regression) | [0, ∞) | Minimize |
| `norm_macro_recall` | Macro recall normalized so random = 0, perfect = 1 | [0, 1] | Maximize |
| `matthews_correlation` | Balanced accuracy metric; works with class imbalance | [-1, 1] | Maximize |
| `precision_score_weighted` | Weighted precision across classes | [0, 1] | Maximize |
| `recall_score_weighted` | Weighted recall across classes | [0, 1] | Maximize |
| `weighted_accuracy` | Accuracy weighted by class sample count | [0, 1] | Maximize |

**Averaging methods for multi-class metrics:**

| Method | How It Works | When to Use |
|---|---|---|
| **Macro** | Compute metric per class, take unweighted average | Class imbalance — gives minority classes equal weight |
| **Micro** | Compute globally by counting total TP, FN, FP | When overall performance matters more than per-class |
| **Weighted** | Compute per class, average weighted by sample count | Standard balanced evaluation |

---

### Regression and Forecasting Metrics

All regression metrics are **normalized** by AutoML for cross-model comparison:
`normalized_error = error / (y_max - y_min)`

| Metric | Description | Range | Objective |
|---|---|---|---|
| `r2_score` | Coefficient of determination — % variance explained | (-∞, 1] | Maximize |
| `normalized_root_mean_squared_error` | RMSE / data range — average error as fraction of range | [0, ∞) | Minimize |
| `normalized_mean_absolute_error` | MAE / data range | [0, ∞) | Minimize |
| `normalized_median_absolute_error` | Median AE / data range — outlier-robust | [0, ∞) | Minimize |
| `spearman_correlation` | Rank correlation between predictions and actuals | [-1, 1] | Maximize |
| `mean_absolute_percentage_error` | Average % difference between predicted and actual | [0, ∞) | Minimize |
| `root_mean_squared_log_error` | RMSE of log-transformed values — undefined if any target = 0 | [0, ∞) | Minimize |

**Regression charts:**

| Chart | What It Shows |
|---|---|
| **Residuals histogram** | Distribution of `y_predicted - y_true`; good model peaks at 0 |
| **Predicted vs. True** | Predicted values binned vs. actual values; ideal = follows `y = x` |
| **Forecast horizon** | (Forecasting only) Predicted vs. actual values over time per CV fold |

---

### Responsible AI Dashboard

AutoML can generate a **Responsible AI (RAI) Dashboard** for the best model (classification and regression only, tabular data):

| RAI Component | What It Provides |
|---|---|
| **Model performance & fairness** | Performance breakdowns across subgroups |
| **Data explorer** | Dataset statistics and distributions |
| **Machine learning interpretability** | Feature importance and SHAP values |
| **Error analysis** | Where and why the model makes mistakes |

> [!NOTE]
> Interpretability (model explanations) is **not available** for forecasting experiments using: TCNForecaster, AutoARIMA, ExponentialSmoothing, Prophet, Average, Naive, SeasonalAverage, or SeasonalNaive.

---

## Linked Reference – MLTable Data Asset

**Source:** [Working with tables in Azure Machine Learning](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-mltable)

**MLTable** is a data asset type in Azure ML that defines:
- **Where** the data lives (local path, Azure Blob, ADLS, public URL)
- **How** to load it (file format, delimiters, column types, transformations)

### MLTable vs. Other Data Asset Types

| Type | Best For | AutoML Compatible? |
|---|---|---|
| **MLTable** | Tabular data with schema definition | ✅ Required for AutoML v2 |
| **URI File** | Single files (CSV, Parquet, image) | ❌ Not directly |
| **URI Folder** | Folders of files | ❌ Not directly |

### Authoring an MLTable

```python
import mltable

# From a local CSV file
tbl = mltable.from_delimited_files(
    paths=[{"file": "./data/train.csv"}]
)

# Optional transformations
tbl = tbl.keep_columns(["age", "income", "label"])
tbl = tbl.convert_column_types({"age": mltable.DataType.to_int()})

# Save the MLTable YAML definition alongside the data
tbl.save("./data/")   # creates ./data/MLTable
```

### Registering as a Data Asset

```python
from azure.ai.ml.entities import Data
from azure.ai.ml.constants import AssetTypes

my_data = Data(
    path="./data/",
    type=AssetTypes.MLTABLE,
    name="diabetes-training-data",
    version="1",
    description="Diabetes dataset for AutoML classification"
)
ml_client.data.create_or_update(my_data)
```

### Using a Registered MLTable in AutoML

```python
from azure.ai.ml import Input
from azure.ai.ml.constants import AssetTypes

training_data = Input(
    type=AssetTypes.MLTABLE,
    path="azureml:diabetes-training-data:1"   # name:version
)
```

---

## Vocabulary / Glossary

| Term | Definition |
|---|---|
| **Automated Machine Learning (AutoML)** | A feature that automatically trains and evaluates multiple ML models with different algorithms and preprocessing steps, finding the best-performing one with minimal manual work. |
| **MLflow** | An open-source platform for managing the ML lifecycle: experiment tracking, model packaging, registry, and deployment. Natively integrated with Azure ML. |
| **MLflow Tracking** | The MLflow component that logs parameters, metrics, tags, and artifacts for each training run, enabling reproducibility and comparison. |
| **MLflow Experiment** | A named container grouping related MLflow runs together (e.g., all training attempts for one project). Default name is `"Default"` if unset. |
| **MLflow Run** | A single execution of model training code tracked by MLflow. Captures parameters, metrics, artifacts, and the trained model. |
| **Autologging** | An MLflow feature that automatically logs parameters, metrics, and models for supported ML frameworks (sklearn, XGBoost, LightGBM, etc.) without manual `log_*` calls. |
| **MLflow Tracking URI** | The endpoint URL directing a local MLflow client to send logged data. For Azure ML, found in the workspace properties page in the Azure portal. |
| **azureml-mlflow** | Python package providing the integration layer between open-source MLflow and Azure ML — handles authentication and routing of tracking data. |
| **MLTable** | An Azure ML data asset type defining a tabular dataset's schema and loading instructions via a YAML file. **Required** input format for AutoML v2 jobs. |
| **Featurization** | Automated feature engineering AutoML applies before training: missing value imputation, categorical encoding, dropping high-cardinality columns, and deriving new features. |
| **Primary Metric** | The evaluation metric AutoML optimizes when selecting the best model (e.g., `accuracy`, `AUC_weighted`). Must be specified in `automl.classification()`. |
| **Data Guardrails** | Automatic data quality checks AutoML runs before training: class imbalance, missing values, and high-cardinality features. |
| **High Cardinality Feature** | A column with a very large number of unique values (e.g., customer IDs, free-text) that typically hurts model generalization. AutoML detects and optionally removes these. |
| **Class Imbalance** | A condition where one target class has far more samples than others (e.g., 95% negative, 5% positive), potentially biasing the model toward the majority class. |
| **Ensemble Model** | A model combining predictions of multiple individual models to improve performance. AutoML may produce VotingEnsemble and StackEnsemble as final candidates. |
| **VotingEnsemble** | Combines predictions via majority vote (classification) or weighted averaging (regression) across multiple base models. |
| **StackEnsemble** | Trains a meta-model (LogisticRegression for classification, ElasticNet for regression) on top of the outputs of multiple base models using Caruana ensemble selection. |
| **MaxAbsScaler** | Scales each feature by dividing by its maximum absolute value → range [-1, 1]. Shown in AutoML model names (e.g., `MaxAbsScaler LightGBM`). |
| **Trial (AutoML)** | A single combination of scaling technique + algorithm + hyperparameters that AutoML trains and evaluates. Many trials run per experiment. |
| **Cross-validation** | A model evaluation technique splitting data into k folds; the model trains on k-1 folds and evaluates on the remaining fold, repeated k times. Reduces overfitting risk. |
| **n_cross_validations** | The number of cross-validation folds (k) to use when evaluating each AutoML trial. Set in `automl.classification()`. AutoML chooses automatically if omitted. |
| **set_limits()** | AutoML method controlling experiment resources: total timeout, per-trial timeout, max trials, and max concurrent trials. |
| **Compute Instance** | A managed cloud VM in Azure ML for development. Notebooks on a compute instance have MLflow pre-configured automatically. |
| **Feature Importance / Interpretability** | Estimates which input features most influenced model predictions. Generated by AutoML via "Generate model explanations" or via the Responsible AI Dashboard. |
| **Confusion Matrix** | A table showing actual vs. predicted class counts, revealing systematic prediction errors. Diagonal = correct, off-diagonal = errors. |
| **ROC Curve** | Plots True Positive Rate vs. False Positive Rate at all thresholds. AUC = probability of ranking a positive above a negative. |
| **AUC** | Area Under the ROC Curve. Range [0,1]; closer to 1 = better. Measures ability to discriminate between classes regardless of threshold. |
| **Precision** | Of all samples predicted positive, what fraction actually are positive. `TP / (TP + FP)`. |
| **Recall (Sensitivity)** | Of all actual positive samples, what fraction the model correctly identified. `TP / (TP + FN)`. |
| **F1 Score** | Harmonic mean of precision and recall. Balances both false positives and false negatives. Range [0,1]. |
| **Calibration Curve** | Plots model confidence vs. actual proportion correct. Well-calibrated model follows y = x. Does not measure classification accuracy. |
| **Lift Curve** | Shows how many times better than random guessing the model is at each confidence threshold. Baseline lift = 1. |
| **Cumulative Gains Curve** | Shows what % of all positives are captured when examining the top X% of predictions ranked by confidence. |
| **RMSE** | Root Mean Squared Error — square root of the mean squared prediction error. Penalizes large errors more than MAE. |
| **R² Score** | Coefficient of determination — proportion of variance in the target explained by the model. Range (-∞, 1]; 1 = perfect. |
| **Distributed Training** | Training a model across multiple compute nodes simultaneously. AutoML supports this for LightGBM (≤1 TB) and TCNForecaster (≤200 GB). |
| **Responsible AI (RAI) Dashboard** | An Azure ML dashboard combining model performance, fairness assessment, data exploration, interpretability, and error analysis for holistic model evaluation. |
| **ONNX** | Open Neural Network Exchange — an open format for ML models enabling deployment across different frameworks and platforms. AutoML can export models to ONNX format. |

---

## Examples

### Example 1 – End-to-End AutoML Classification Job

```python
from azure.ai.ml import MLClient, automl, Input
from azure.ai.ml.constants import AssetTypes
from azure.identity import DefaultAzureCredential

# 1. Connect to workspace
ml_client = MLClient(
    DefaultAzureCredential(),
    subscription_id="<subscription-id>",
    resource_group_name="<resource-group>",
    workspace_name="<workspace>"
)

# 2. Reference registered MLTable data asset
training_data = Input(
    type=AssetTypes.MLTABLE,
    path="azureml:diabetes-training-data:1"
)

# 3. Configure classification job
automl_job = automl.classification(
    compute="cpu-cluster",
    experiment_name="diabetes-automl-classification",
    training_data=training_data,
    target_column_name="Diabetic",
    primary_metric="accuracy",
    n_cross_validations=5,
    enable_model_explainability=True
)

# 4. Set resource limits
automl_job.set_limits(
    timeout_minutes=60,
    trial_timeout_minutes=15,
    max_trials=10,
    max_concurrent_trials=4,
    enable_early_termination=True
)

# 5. Block algorithms you don't want
automl_job.set_training(
    blocked_training_algorithms=["NaiveBayes"]
)

# 6. Submit and monitor
returned_job = ml_client.jobs.create_or_update(automl_job)
print("Monitor at:", returned_job.studio_url)
```

---

### Example 2 – Register an MLTable Data Asset

```python
import mltable
from azure.ai.ml.entities import Data
from azure.ai.ml.constants import AssetTypes

# Build MLTable from local CSV
tbl = mltable.from_delimited_files(paths=[{"file": "./data/diabetes.csv"}])
tbl.save("./data/")   # writes ./data/MLTable YAML

# Register with Azure ML
my_data = Data(
    path="./data/",
    type=AssetTypes.MLTABLE,
    name="diabetes-training-data",
    version="1",
    description="Diabetes training data for classification"
)
ml_client.data.create_or_update(my_data)
print("Data asset registered successfully.")
```

---

### Example 3 – MLflow Autologging

```python
import mlflow
import mlflow.sklearn
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split
import pandas as pd

# Load and split data
df = pd.read_csv("diabetes.csv")
X = df.drop("Diabetic", axis=1)
y = df["Diabetic"]
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Set experiment and enable autologging
mlflow.set_experiment("diabetes-rf-experiment")
mlflow.sklearn.autolog()

# Train — MLflow logs everything automatically
with mlflow.start_run(run_name="rf-100trees"):
    model = RandomForestClassifier(n_estimators=100, max_depth=5, random_state=42)
    model.fit(X_train, y_train)
    # Auto-logged: n_estimators, max_depth, accuracy, F1, AUC, precision, recall, model file
```

---

### Example 4 – MLflow Custom Logging with Artifact

```python
import mlflow
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score, roc_auc_score, ConfusionMatrixDisplay
import matplotlib.pyplot as plt

mlflow.set_experiment("diabetes-lr-experiment")

with mlflow.start_run(run_name="logreg-C0.1"):
    # Train
    C = 0.1
    model = LogisticRegression(C=C, max_iter=200, solver="liblinear")
    model.fit(X_train, y_train)

    # Log hyperparameters
    mlflow.log_param("C", C)
    mlflow.log_param("max_iter", 200)
    mlflow.log_param("solver", "liblinear")

    # Log metrics
    acc = accuracy_score(y_test, model.predict(X_test))
    auc = roc_auc_score(y_test, model.predict_proba(X_test)[:, 1])
    mlflow.log_metric("accuracy", acc)
    mlflow.log_metric("AUC", auc)

    # Log confusion matrix as artifact
    fig, ax = plt.subplots()
    ConfusionMatrixDisplay.from_estimator(model, X_test, y_test, ax=ax)
    fig.savefig("confusion_matrix.png")
    mlflow.log_artifact("confusion_matrix.png")

    # Log the model
    mlflow.sklearn.log_model(model, artifact_path="logistic-regression-model")

    # Add tags for filtering
    mlflow.set_tag("framework", "sklearn")
    mlflow.set_tag("dataset", "diabetes-v1")

print(f"Run complete | Accuracy: {acc:.4f} | AUC: {auc:.4f}")
```

---

### Example 5 – Comparing Runs in Azure ML Studio

After training multiple models:

1. Navigate to **Jobs** → **Experiments** in AML Studio
2. Select your experiment (e.g., `diabetes-lr-experiment`)
3. Check boxes next to 2+ runs in the table
4. Click **Compare** → side-by-side chart of all logged metrics
5. Sort by any column (accuracy, AUC, etc.) to find the best run
6. Click **Register model** on the best run → promotes it for deployment

---

### Example 6 – Distributed AutoML for Large Datasets

```python
# For datasets > 1 GB, use distributed LightGBM
automl_job = automl.classification(
    compute="large-cluster",
    experiment_name="large-scale-automl",
    training_data=training_data,
    target_column_name="label",
    primary_metric="AUC_weighted"
)

automl_job.set_training(
    training_mode="distributed"   # enable distributed training
)

automl_job.set_limits(
    timeout_minutes=120,
    max_nodes=8                   # cluster nodes to use (min 4)
)
```

---

## Key Takeaways

> [!SUMMARY]
> - **AutoML** = best when you need to quickly benchmark many algorithms with minimal code
> - **MLflow** = best when iterating manually in notebooks and need systematic tracking
> - Both store results in **Azure ML Studio**, enabling consistent comparison and governance
> - **MLTable** is the required data format for AutoML v2 — register your dataset asset before submitting
> - Always set **`set_limits()`** parameters (`timeout_minutes`, `max_trials`) to control cloud compute costs
> - For **imbalanced datasets**, prefer `AUC_weighted` over `accuracy` as the primary metric
> - For datasets **> 20,000 rows**, AutoML uses a 10% holdout; for smaller datasets it uses cross-validation automatically
> - **Autologging** is the fastest way to start with MLflow; add **custom logging** for business metrics and plots
> - A Data Guardrail **"Alerted"** status means you should investigate and fix the data quality issue before trusting results
> - Algorithm names follow the pattern: `ScalingMethod AlgorithmName` (e.g., `MaxAbsScaler LightGBM`)
> - **Ensemble models** (VotingEnsemble, StackEnsemble) often rank near the top — AutoML enables them by default
> - **Distributed training** supports only LightGBM (≤1 TB) and TCNForecaster (≤200 GB); does not support ensembles or ONNX in distributed mode
> - **Model interpretability** is unavailable for certain forecasting algorithms (TCNForecaster, AutoARIMA, Prophet, ExponentialSmoothing, Naive variants)

---

## Related Notes

- [[Azure Machine Learning]]
- [[Python SDK v2]]
- [[MLflow]]
- [[Classification Models]]
- [[Feature Engineering]]
- [[Model Evaluation Metrics]]
- [[MLTable]]
- [[Compute Clusters]]
- [[Azure ML Studio]]
- [[Model Interpretability]]
- [[Responsible AI]]
- [[Ensemble Methods]]
- [[Cross-Validation]]
- [[Scikit-Learn]]
- [[XGBoost]]
- [[LightGBM]]
