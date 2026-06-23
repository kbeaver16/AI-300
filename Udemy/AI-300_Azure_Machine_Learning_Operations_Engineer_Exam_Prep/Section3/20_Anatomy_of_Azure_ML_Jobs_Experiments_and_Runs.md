# 20 – Anatomy of Azure ML Jobs, Experiments and Runs

---

## 📌 Key Concepts

- **Experiments:** logical containers that group related ML training jobs together
- **Jobs:** the fundamental execution units within an experiment; track artifacts, metrics, and hyperparameters
- **Runs:** individual executions of a job submitted from code (notebooks) via MLflow
- **MLflow:** open-source library deeply integrated into Azure ML for experiment tracking, metric logging, and model file generation
- **Job Dashboard:** the UI view of a completed job — shows hyperparameters, metrics (accuracy, precision, recall), and output artifacts
- **Model Registry:** where model files from completed jobs are registered before deployment
- **Hyperparameter Logging:** MLflow captures each hyperparameter value alongside its resulting metrics per job
- **Outputs + Logs:** every job stores stdout logs, model files, and evaluation metrics accessible from the job detail view

---

## 📖 Definitions

| Term | Definition |
|------|------------|
| Experiment | A logical container in Azure ML that organises and tracks related ML jobs |
| Job | The execution unit of an experiment run; stores model artifacts, metrics, and hyperparameters |
| Run | An individual submission of a job from a notebook/code environment; captured by MLflow |
| MLflow | Open-source ML lifecycle library integrated with Azure ML; enables experiment tracking and metric logging |
| Hyperparameter | Tuning parameter set before training (e.g. learning rate, lambda); logged per job by MLflow |
| Model Artifact | The output files of a trained model (weights, schema, scoring script) stored in the job |
| Model Registry | Azure ML component where trained models are registered for versioning and deployment |
| Outputs + Logs | Job section containing stdout logs (for debugging) and trained model output files |
| Latest Job | The most recent job within an experiment; shown as the primary row in the experiment list |

---

## 💡 Examples

- **Multiple experiments** visible in the Jobs view — each representing a different model type or problem
- **Within one experiment:** multiple jobs showing runs with different algorithms or hyperparameter values (e.g. α=10, α=0.5)
- **Job dashboard metrics displayed:** accuracy, precision, recall — logged for a classification model
- **Failed jobs** are visible alongside completed ones within the same experiment — both accessible for debugging

---

## 🔢 Step-by-Step Processes

### Navigating the Jobs / Experiments / Runs Hierarchy

1. Go to **Assets → Jobs** in Azure ML workspace
2. View the list of **Experiments** — each row represents one model development effort
3. Click an experiment → view its list of **Jobs** (individual runs)
4. Click a job → view the **Job Dashboard**:
   - **Overview tab:** hyperparameters used, algorithm selected, summary metrics
   - **Metrics tab:** accuracy, precision, recall, MSE logged via MLflow
   - **Outputs + Logs tab:** access model artifact files and stdout debug logs
5. From the model artifacts, click **Register Model** to push to the **Model Registry**
6. From the Model Registry, deploy to a real-time or batch endpoint

### Submitting a Run from a Notebook (Code-First Pattern)
1. Import `mlflow` in your notebook
2. Start a run: `mlflow.start_run(experiment_name="my_experiment")`
3. Log hyperparameters: `mlflow.log_param("alpha", 0.5)`
4. Log metrics: `mlflow.log_metric("accuracy", 0.87)`
5. Log model: `mlflow.sklearn.log_model(model, "model")`
6. End run: `mlflow.end_run()`
7. Navigate to **Jobs → Experiment** in Azure ML Studio to view the logged run

---

## 💻 Code Blocks

```python
import mlflow
import mlflow.sklearn
from azureml.core import Workspace

# Connect to Azure ML workspace
ws = Workspace.from_config()
mlflow.set_tracking_uri(ws.get_mlflow_tracking_uri())

# Set experiment name
mlflow.set_experiment("diabetes_prediction")

with mlflow.start_run():
    # Log hyperparameters
    mlflow.log_param("max_iter", 100)
    mlflow.log_param("C", 0.3)

    # Train model (example)
    model.fit(X_train, y_train)

    # Log evaluation metrics
    mlflow.log_metric("accuracy", accuracy_score(y_test, y_pred))
    mlflow.log_metric("precision", precision_score(y_test, y_pred))
    mlflow.log_metric("recall", recall_score(y_test, y_pred))

    # Log the model artifact
    mlflow.sklearn.log_model(model, "model")
```

---

## 📝 Summary

> "Jobs in Azure machine learning are the fundamental execution units used to run machine learning tasks such as training models, data processing or pipelines on compute resources within an Azure ML workspace."

This lecture clarifies the three-tier hierarchy: **Experiments** group logically related model development efforts; **Jobs** are the individual execution instances within an experiment, each capturing hyperparameters, metrics, and model artifacts; **Runs** are the programmatic submission events (via MLflow from notebooks) that create those jobs. Understanding this hierarchy is essential for comparing model variants, debugging failures, and promoting the best model to the registry for deployment.

---

## ✅ Actionable Takeaways

- Always create a **new experiment** for each distinct ML problem or model type — don't reuse experiments across unrelated models
- Use **MLflow** to log hyperparameters, metrics, and model artifacts in every training run
- Use the **Outputs + Logs → stdout log** to debug failed job components
- After a successful job, **register the model** from the job artifacts before deploying
- Navigate **Experiment → Job → Overview tab** to compare hyperparameter/metric combinations across runs
- Understand that a **run (code submission) creates a job (execution record)** within an experiment

---

## 🏷️ Tags

`#azure-ml` `#experiments` `#jobs` `#runs` `#mlflow` `#model-registry` `#hyperparameters` `#model-tracking` `#lifecycle` `#devops-ml` `#logging` `#artifacts`

---

## 🔗 Backlink Suggestions

- [[17 – Lab Training a Cost Function Prediction Model with Designer]] — the automobile pipeline was submitted as an experiment + job
- [[18 – Lab Creating a Realtime Inference Pipeline]] — the inference pipeline was a separate experiment
- [[21 – Lab Training a Diabetes Predictor Model]] — notebook-based run submission via the Azure ML client
- [[23 – Introduction to AutoML]] — AutoML creates experiments and jobs automatically
- [[24 – Lab Automated Training with AutoML]] — AutoML jobs are visible in the same Jobs → Experiments view
