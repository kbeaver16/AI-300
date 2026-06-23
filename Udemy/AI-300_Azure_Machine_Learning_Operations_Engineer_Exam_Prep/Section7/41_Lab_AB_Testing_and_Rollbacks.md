# Lab: Implementing A/B Testing and Rollbacks

## Key Concepts
- Existing Endpoints
- Secondary Deployments
- Traffic Allocation
- Rollback Operations

## Definitions
### Deployment
A model running behind an endpoint.

### Endpoint
A single URL capable of routing requests to multiple deployments.

## Examples
- MLflow diabetes model.
- Automobile price prediction model.

## Step-by-Step Processes
### Create an A/B Test
1. Open endpoint configuration.
2. Select a model from the registry.
3. Deploy to an existing endpoint.
4. Configure scoring script.
5. Select environment.
6. Select compute target.
7. Configure traffic split.
8. Deploy.

### Perform a Rollback
1. Review deployment performance.
2. Update traffic allocation.
3. Route traffic away from failing deployment.
4. Send 100% traffic to stable deployment.
5. Remove poor-performing deployment.

## Code Blocks
No code examples provided in the transcript.

## Summary
The lab demonstrates configuring multiple deployments behind a single endpoint, adjusting traffic distribution, and performing rollback operations through traffic management. citeturn4search15

## Actionable Takeaways
- Create rollback plans before deployment.
- Monitor deployment quality continuously.
- Use traffic controls for safe experimentation.

## Tags
#AI300 #ABTesting #Rollback #AzureML #Endpoints

## Backlink Suggestions
- [[A/B Testing]]
- [[Model Deployment]]
- [[Azure ML Endpoints]]
