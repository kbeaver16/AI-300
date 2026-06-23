# Lab - Deploying an LLM Model in Azure ML Workspace

## Summary
Walkthrough of creating a Microsoft Foundry resource, connecting it to Azure ML, and deploying GPT and Anthropic models through the Azure ML Model Catalog.

## Key Concepts
- Microsoft Foundry Resource
- Azure ML Model Catalog
- Azure AI User Role
- Connections Management Center
- Chat Playground
- Model Deployment

## Definitions
### Microsoft Foundry
Microsoft's platform for building and managing generative AI applications and model deployments.

### Azure AI User Role
Azure role providing access to AI projects and AI resources.

### Connections Management Center
Area of Azure ML used to connect external AI resources, storage, services, and Foundry projects.

## Step-by-Step Process
### Create a Microsoft Foundry Resource
1. Open Azure Portal.
2. Search for Microsoft Foundry.
3. Create a standalone Foundry project.
4. Select resource group.
5. Configure region.
6. Create the resource.

### Configure Permissions
1. Open Access Control (IAM).
2. Add Azure AI User Role.
3. Assign role to your user.
4. Verify access in Foundry Studio.

### Connect Foundry to Azure ML
1. Open Azure ML Workspace.
2. Navigate to Connections Management Center.
3. Select Connect.
4. Choose Microsoft Foundry Resource.
5. Authenticate using API Key.
6. Add connection.

### Deploy a Model
1. Open Model Catalog.
2. Select a model (GPT-4.1, Claude, etc.).
3. Choose Foundry connection.
4. Provide deployment name.
5. Deploy model.
6. Test in Foundry Chat Playground.

## Examples
- GPT deployment.
- Anthropic model deployment.
- Chat playground testing.

## Deployment Types Referenced
- Global Standard
- Data Zone Standard
- Global Batch
- Data Zone Batch
- Provisioned Throughput

## Code Blocks
No code examples were included in the transcript.

## Actionable Takeaways
- Create Foundry before deploying models from Azure ML.
- Ensure correct Azure role assignments.
- Use Connections Management Center to integrate services.
- Validate deployments using Chat Playground.

## Backlink Suggestions
- [[Azure Machine Learning]]
- [[Microsoft Foundry]]
- [[Model Catalog]]
- [[Azure IAM]]
- [[LLM Deployment]]

## Tags
#AI300 #AzureML #Foundry #GPT4 #Claude #ModelDeployment #AzureAI
