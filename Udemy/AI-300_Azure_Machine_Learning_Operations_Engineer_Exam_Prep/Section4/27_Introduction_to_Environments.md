# Introduction to Azure ML Environments

## Key Concepts
- Curated environments
- Custom environments
- Dependency management
- Docker-based execution
- Azure Container Registry integration

## Definitions
### Environment
A collection of software packages, libraries, and dependencies required to run training or inference workloads.

### Curated Environment
A Microsoft-maintained, prebuilt environment.

### Custom Environment
A user-defined environment with specific dependencies.

### Conda File
Defines Python packages and dependency requirements.

## Examples
- Curated environments containing PyTorch, TensorFlow, or Scikit-Learn.
- Custom environments extending curated images.

## Step-by-Step Process
1. Select a curated base image.
2. Create a conda.yaml file.
3. Define dependencies.
4. Create a Docker configuration.
5. Build the environment.
6. Store the image in Azure Container Registry.
7. Version the environment.
8. Deploy models using the environment.

## Code Blocks
```yaml
name: custom-env
channels:
  - conda-forge
dependencies:
  - python=3.10
```

## Summary
Environments provide reproducible execution for training and inference. Azure ML supports curated and custom environments, with versioned storage in Azure Container Registry.

## Actionable Takeaways
- Use curated environments when possible.
- Create custom environments for specialized dependencies.
- Reuse MLflow-generated conda files.
- Version environments for rollback scenarios.

## Tags
#AI300 #AzureML #Environments #Docker #ACR

## Backlink Suggestions
- [[Azure Container Registry]]
- [[MLflow]]
- [[Model Deployment]]
- [[Docker]]
