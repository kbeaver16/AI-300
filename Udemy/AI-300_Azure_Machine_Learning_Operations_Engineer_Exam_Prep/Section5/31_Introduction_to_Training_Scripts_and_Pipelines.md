# Introduction to Training Scripts and Pipelines

## Key Concepts
- Azure ML training scripts
- Jupyter Notebook to Python script conversion
- Reusability and standardization
- Pipeline orchestration
- Components and compute targets
- Datastores for state management

## Definitions
### Training Script
A reusable Python script created from notebook code to execute model training in a standardized way.

### Component
A reusable unit in Azure Machine Learning that can be incorporated into a pipeline.

### Pipeline
A sequence of connected components that automate the machine learning lifecycle.

### Datastore
A storage location used by Azure ML pipelines to persist state and intermediate outputs.

## Examples
- Converting a successful notebook experiment into a Python script.
- Creating separate scripts for feature engineering, training, evaluation, and model registration.
- Connecting scripts together in a pipeline.

## Step-by-Step Process
1. Experiment in Jupyter Notebooks.
2. Validate model performance.
3. Convert notebook logic into Python scripts.
4. Register scripts as pipeline components.
5. Connect component outputs to downstream inputs.
6. Configure compute resources.
7. Configure a datastore.
8. Define pipeline relationships in YAML.
9. Submit the pipeline as a job.
10. Monitor execution and outputs.

## Code Blocks
```yaml
jobs:
  prep_data:
    outputs:
      prepared_data:

  train_model:
    inputs:
      prepared_data: ${{parent.jobs.prep_data.outputs.prepared_data}}
```

## Summary
Training scripts transform exploratory notebook work into reusable automation. Pipelines connect multiple scripts together to create repeatable end-to-end ML workflows.

## Actionable Takeaways
- Convert stable notebooks into scripts.
- Design scripts around a single responsibility.
- Register scripts as reusable components.
- Use pipelines to automate the full ML lifecycle.

## Tags
#AI300 #AzureML #TrainingScripts #Pipelines #MLOps

## Backlink Suggestions
- [[Azure Machine Learning]]
- [[MLOps Lifecycle]]
- [[ML Pipelines]]
- [[Training Scripts]]
