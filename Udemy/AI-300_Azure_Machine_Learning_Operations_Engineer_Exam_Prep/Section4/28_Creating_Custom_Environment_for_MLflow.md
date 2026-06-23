# Lab: Creating a Custom Environment for MLflow Deployment

## Key Concepts
- Custom environment creation
- Parent images
- Conda dependency specification
- Environment versioning

## Definitions
### Parent Image
The base Docker image used to create a custom environment.

### Environment Version
A tracked revision of an environment configuration.

## Examples
- Using Microsoft's MLflow inference image.
- Creating an Iris environment from a curated environment.

## Step-by-Step Process
1. Navigate to Azure ML Environments.
2. Create a new custom environment.
3. Choose an existing Docker image.
4. Use the MLflow inference base image.
5. Create or paste a conda.yaml file.
6. Define Python and package dependencies.
7. Create the environment.
8. Monitor build logs.
9. Verify image creation in ACR.
10. Confirm environment version availability.

## Code Blocks
```yaml
name: iris
dependencies:
  - python=3.10
  - pip:
      - mlflow
      - scikit-learn
      - pandas
```

## Summary
The lab demonstrates building an Azure ML custom environment from a curated image and storing the resulting container image in Azure Container Registry.

## Actionable Takeaways
- Start from curated images to reduce effort.
- Verify build logs when environment creation fails.
- Keep environment versions aligned with model versions.

## Tags
#AI300 #AzureML #MLflow #CustomEnvironment #ACR

## Backlink Suggestions
- [[Azure Container Registry]]
- [[MLflow Deployment]]
- [[Azure ML Environments]]
