# Model Deployment Types in Azure Machine Learning

## Key Concepts
- Real-time Endpoints
- Batch Endpoints
- Web Service Endpoints
- Model Artifacts
- Environments
- Compute Targets

## Definitions
### Real-time Endpoint
Endpoint designed for low-latency inference requests.

### Batch Endpoint
Endpoint used for large-scale batch prediction workloads.

### Web Service Endpoint
A model deployment exposed through Azure Container Instances (ACI) or Azure Kubernetes Service (AKS).

### Compute Target
Infrastructure used to execute model inference.

## Examples
- Diabetes MLflow model deployed to a managed online endpoint.
- Models deployed to AKS or ACI.
- Batch scoring against data stored in cloud storage.

## Step-by-Step Processes
### Real-Time Deployment
1. Register model.
2. Create environment.
3. Select compute target.
4. Create endpoint.
5. Deploy model.
6. Send API requests.

### Batch Deployment
1. Store input data.
2. Configure batch endpoint.
3. Submit batch job.
4. Generate predictions.
5. Write output to storage.

## Code Blocks
No code examples provided in the transcript.

## Summary
Azure ML supports real-time, batch, and web-service deployments. Each deployment model targets different performance, scalability, and operational requirements. citeturn4search13

## Actionable Takeaways
- Use real-time endpoints for low-latency inference.
- Use batch endpoints for large-scale scoring.
- Secure endpoints through authentication and network controls.

## Tags
#AI300 #AzureML #ModelDeployment #BatchEndpoints #RealTimeEndpoints

## Backlink Suggestions
- [[MLflow Deployment]]
- [[Azure ML Endpoints]]
- [[Model Registry]]
- [[Custom Environments]]
