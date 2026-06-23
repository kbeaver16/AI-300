# A/B Testing and Rollback Strategies

## Key Concepts
- A/B Testing
- Traffic Splitting
- Multiple Deployments
- Rollbacks
- Model Versioning

## Definitions
### A/B Testing
A technique for comparing two model deployments by splitting incoming traffic.

### Rollback
Reverting traffic back to a previously trusted deployment.

### Traffic Allocation
The percentage of endpoint requests routed to a deployment.

## Examples
- E-commerce checkout-page experiments.
- Comparing multiple versions of a machine learning model.

## Step-by-Step Processes
### A/B Testing
1. Deploy multiple models behind one endpoint.
2. Configure traffic percentages.
3. Monitor outcomes.
4. Compare performance.
5. Select the preferred deployment.

### Rollback
1. Maintain model versions.
2. Identify a problematic deployment.
3. Redirect traffic to a stable version.
4. Reduce faulty deployment traffic to 0%.
5. Remove the deployment if required.

## Code Blocks
No code examples provided in the transcript.

## Summary
Azure ML supports A/B testing by allowing multiple deployments behind the same endpoint and routing configurable percentages of traffic to each deployment. Rollbacks are implemented through traffic reassignment and model versioning. citeturn4search14

## Actionable Takeaways
- Always version models.
- Test new deployments with limited traffic.
- Maintain rollback readiness.

## Tags
#AI300 #ABTesting #AzureML #Rollback #MLOps

## Backlink Suggestions
- [[Model Registry]]
- [[Azure ML Endpoints]]
- [[Traffic Allocation]]
