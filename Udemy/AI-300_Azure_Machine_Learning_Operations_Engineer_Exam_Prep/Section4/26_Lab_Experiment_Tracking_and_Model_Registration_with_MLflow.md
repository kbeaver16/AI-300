# Experiment Tracking & Model Registration with MLflow (Lab)

## Key Concepts

- **MLflow Experiment Tracking** — Logs parameters, metrics, artifacts, and source code for each training run.
- **MLflow Runs** — Individual jobs executed under an experiment.
- **Model Artifacts** — Files generated during training (model pickle, conda.yaml, python_env.yaml, requirements.txt, input example).
- **Model Signature** — Defines expected input and output schema for inference.
- **Azure ML Integration** — MLflow is natively supported in Azure ML compute instances.
- **Model Registry** — Stores versioned MLflow models for deployment.

---

## Definitions

- **Experiment** — A container for multiple MLflow runs.
- **Run** — A single execution of training code with logged metadata.
- **Hyperparameters** — Configurable values such as `max_iter` and `regularization` for logistic regression.
- **Metrics** — Evaluation outputs such as accuracy, precision, recall.
- **Artifact Path** — Directory where MLflow stores model files.
- **Input Example** — JSON defining the expected input structure for deployed models.

---

## Examples

### Example: Hyperparameters Logged

- `max_iter = 100`
- `regularization = 0.2`

### Example: Metrics Logged

- Accuracy: **73.1%**
- Weighted Precision: **73.0%**
- Weighted Recall: **73.1%**

### Example: Model Artifacts

- `conda.yaml`
- `python_env.yaml`
- `requirements.txt`
- `MLmodel`
- `model.pkl`
- `input_example.json`

---

## Step‑by‑Step Processes

### 1. Prepare MLflow-Compatible Environment

1. Install MLflow versions compatible with Azure ML (MLflow v3 not supported).
2. Restart the Python kernel to apply changes.
3. Select the correct kernel (Azure ML SDK v2).

### 2. Load Data

1. Use Azure ML client to load the **diabetes MLTable**.
2. Convert MLTable to a pandas DataFrame.
3. Inspect feature columns and target (`Outcome`).

### 3. Preprocess Data

1. Select feature columns.
2. Apply **MinMaxScaler** to normalize values.
3. Split into train/test sets (70/30, `random_state=42`).

### 4. Create an MLflow Experiment

```python
import mlflow
mlflow.set_experiment("MLflow-experiment")
```

### 5. Define Training Function with MLflow Logging

- Start a run:

```python
with mlflow.start_run():
```

- Log hyperparameters:

```python
mlflow.log_param("max_iter", max_iter)
mlflow.log_param("regularization", reg)
```

- Train logistic regression model.
- Compute metrics:

```python
mlflow.log_metric("accuracy", accuracy)
mlflow.log_metric("precision_weighted", precision)
mlflow.log_metric("recall_weighted", recall)
```

- Define model signature (input + output schema).
- Log model:

```python
mlflow.sklearn.log_model(
    model,
    artifact_path="model",
    signature=signature,
    input_example=input_example
)
```

- End run:

```python
mlflow.end_run()
```

### 6. Compare Runs in Azure ML

1. Navigate to **Jobs → MLflow Experiment**.
2. Inspect each run’s:
    - Parameters
    - Metrics
    - Artifacts
3. Identify best-performing model (in this lab: `max_iter=100`, `regularization=0.2`).

### 7. Register the Best Model

1. Select the best run.
2. Choose **Register Model**.
3. Select **MLflow model** type.
4. Point to the `model/` artifact folder.
5. Name the model (e.g., `diabetes-mlflow`).
6. Register → Version 1 created.

### 8. Validate Model in Registry

- Confirm all artifacts are present.
- Model is now deployable to:
    - Real-time endpoint
    - Batch endpoint
    - AKS / ACI / Serverless

---

## Code Blocks

### MLflow Experiment Setup

```python
import mlflow
mlflow.set_experiment("MLflow-experiment")
```

### Logging Parameters & Metrics

```python
with mlflow.start_run():
    mlflow.log_param("max_iter", max_iter)
    mlflow.log_param("regularization", reg)

    model = LogisticRegression(max_iter=max_iter, C=reg)
    model.fit(X_train, y_train)

    preds = model.predict(X_test)
    mlflow.log_metric("accuracy", accuracy_score(y_test, preds))
```

### Logging Model with Signature

```python
from mlflow.models.signature import infer_signature

signature = infer_signature(X_train, model.predict(X_train))

mlflow.sklearn.log_model(
    model,
    artifact_path="model",
    signature=signature,
    input_example=X_train.iloc[:1]
)
```

---

## Summary

This lab demonstrates how to use MLflow within Azure ML to track experiments, log hyperparameters and metrics, store model artifacts, and register the best-performing model. By running multiple training jobs with different parameter combinations, you can compare performance and promote the best model into the Azure ML Model Registry for deployment. MLflow’s integration with Azure ML makes experiment tracking and model management seamless and reproducible.

---

## Actionable Takeaways

- Always start experiments with `mlflow.set_experiment()`.
- Log **both** hyperparameters and metrics for reproducibility.
- Use MLflow’s **signature** to ensure correct inference schema.
- Compare runs in Azure ML UI to select best-performing models.
- Register MLflow models directly from job artifacts.
- Use the `model/` artifact folder for deployment-ready assets.

---

## Tags

#mlflow #azureml #experiment‑tracking #model‑registry #mlops #sklearn #diabetes-model

---

## Backlink Suggestions

- [[MLflow – Introduction]]
- [[Azure ML – Experiments & Jobs]]
- [[Model Registry – Best Practices]]
- [[Real‑Time Endpoints in Azure ML]]
- [[Hyperparameter Tuning Notes]]
- [[MLOps Lifecycle Overview]]