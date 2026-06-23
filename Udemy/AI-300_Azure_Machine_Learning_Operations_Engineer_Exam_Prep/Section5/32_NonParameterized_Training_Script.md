# Lab: Non-Parameterized Training Script

## Key Concepts
- Script-based model training
- MLflow integration
- Logistic Regression
- Curated environments
- Compute instances

## Definitions
### Non-Parameterized Script
A training script whose configuration values are hard-coded.

### Curated Environment
A Microsoft-managed environment that contains preconfigured ML dependencies.

### Pickle Model
A serialized Python model stored as a .pkl file.

## Examples
- Training a diabetes prediction model.
- Logging metrics and parameters using MLflow.
- Saving a diabetes_model.pkl artifact.

## Step-by-Step Process
1. Create an Azure ML client.
2. Create a diabetes training folder.
3. Download diabetes.csv.
4. Create a Python training script.
5. Start an MLflow run.
6. Load the CSV dataset.
7. Scale features using MinMaxScaler.
8. Split training and testing datasets.
9. Train a Logistic Regression model.
10. Log parameters.
11. Calculate accuracy, precision, and recall.
12. Log metrics.
13. Save a pickle model file.
14. Execute the script on a compute instance.
15. Review results in Azure ML jobs.

## Code Blocks
```python
mlflow.start_run()

model = LogisticRegression()
model.fit(X_train, y_train)

mlflow.log_metric('accuracy', accuracy)
```

## Summary
This lab demonstrates how to move model training into a standalone Python script and execute it through Azure Machine Learning while tracking results with MLflow.

## Actionable Takeaways
- Use scripts instead of notebooks for repeatable workloads.
- Log all metrics with MLflow.
- Store trained models as artifacts.
- Execute scripts through managed compute.

## Tags
#AI300 #AzureML #MLflow #TrainingScripts #LogisticRegression

## Backlink Suggestions
- [[MLflow]]
- [[Training Scripts]]
- [[Logistic Regression]]
- [[Model Artifacts]]
