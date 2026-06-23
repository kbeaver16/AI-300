# Lab: Build and Execute an Azure ML Pipeline

## Key Concepts
- Pipeline components
- Data preparation
- Model training
- Pipeline orchestration
- MLflow monitoring

## Definitions
### Pipeline Component
A reusable execution unit within a pipeline.

### Prep Script
A component responsible for cleaning and preparing data.

### Training Component
A component that trains and evaluates a model.

### ROC Curve
A visualization showing model classification performance.

## Examples
- prep_diabetes.py for preprocessing.
- train_diabetes.py for model training.
- Diabetes CSV data asset feeding the pipeline.

## Step-by-Step Process
1. Create an Azure ML client.
2. Build a pipeline workspace folder.
3. Create a data preparation script.
4. Create a training script.
5. Configure environments.
6. Configure compute resources.
7. Register scripts as components.
8. Define component inputs and outputs.
9. Connect prep output to training input.
10. Create the pipeline definition.
11. Submit the pipeline job.
12. Monitor nested jobs.
13. Review metrics and artifacts.
14. Register the resulting model.

## Code Blocks
```python
@pipeline(description='Diabetes Training Pipeline')
def diabetes_pipeline():
    prep_step = prep_component(...)
    train_step = train_component(
        training_folder=prep_step.outputs.prepped_data
    )
```

```python
mlflow.log_metric('accuracy', accuracy)
mlflow.log_metric('auc', auc)
```

## Summary
The lab demonstrates creating a multi-stage Azure ML pipeline that prepares data, trains a model, evaluates performance, logs artifacts, and produces a deployable model.

## Actionable Takeaways
- Break workflows into reusable components.
- Store intermediate outputs in datastores.
- Monitor individual pipeline stages separately.
- Log evaluation metrics and visualizations.

## Tags
#AI300 #AzureML #Pipelines #MLflow #MLOps #DataPreparation

## Backlink Suggestions
- [[Azure ML Pipelines]]
- [[Training Scripts]]
- [[MLflow]]
- [[ROC Curve]]
- [[Model Evaluation]]
