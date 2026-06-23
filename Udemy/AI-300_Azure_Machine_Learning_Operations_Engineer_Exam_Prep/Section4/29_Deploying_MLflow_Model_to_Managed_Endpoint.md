# Lab: Deploying an MLflow Model to a Managed Endpoint

## Key Concepts
- Managed online endpoints
- Deployments
- Scoring scripts
- Authentication
- Traffic allocation

## Definitions
### Endpoint
An API surface that exposes a model for inference.

### Deployment
A model instance hosted behind an endpoint.

### Scoring Script
Code that processes requests and generates predictions.

## Examples
- Deploying a diabetes prediction model.
- Returning True/False diabetes risk predictions.

## Step-by-Step Process
1. Open the model registry.
2. Select an MLflow model.
3. Create a managed online endpoint.
4. Configure authentication.
5. Create a deployment.
6. Upload a score.py file.
7. Select a custom environment.
8. Choose compute resources.
9. Configure traffic allocation.
10. Deploy the endpoint.
11. Retrieve endpoint URL and keys.
12. Test using HTTP requests.

## Code Blocks
```python
result = model.predict(input_df)
```

```json
{
  "predictions": [1]
}
```

## Summary
The lab walks through deploying an MLflow model behind a managed endpoint, configuring compute, uploading a scoring script, and validating predictions through API calls.

## Actionable Takeaways
- Separate endpoint and deployment concepts.
- Use custom scoring scripts to control inference logic.
- Monitor endpoint health and traffic allocation.
- Secure endpoints with authentication keys.

## Tags
#AI300 #AzureML #MLflow #Endpoint #Deployment #Inference

## Backlink Suggestions
- [[MLflow]]
- [[Azure ML Endpoints]]
- [[Scoring Scripts]]
- [[Model Deployment]]
