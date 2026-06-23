# Introduction to GenAI on Azure ML Workspace

## Summary
Overview of generative AI capabilities in Azure Machine Learning (Azure ML), including Prompt Flows, Model Catalog, GenAI Management Center, and evaluation capabilities.

## Key Concepts
- Prompt Flows
- Model Catalog
- Serverless API Inferencing
- Microsoft Foundry Resource
- GenAI Management Center
- Automated Evaluations
- Custom Evaluations
- GenAIOps Lifecycle

## Definitions
### Prompt Flow
A containerized generative AI microservice built primarily through prompt engineering and lightweight Python scripting.

### Model Catalog
A repository of foundation models from multiple providers including OpenAI and Anthropic that can be deployed from Azure ML.

### Serverless API Inferencing
A deployment model where you pay only for model execution (token usage) rather than dedicated infrastructure.

### GenAI Management Center
The Azure ML area used to manage connections to AI services and Microsoft Foundry resources.

## Examples
- Deploying OpenAI models from the Model Catalog.
- Connecting Azure AI Services through the Connections tab.
- Evaluating chatbot groundedness and relevance.

## Step-by-Step Process
### Build a GenAI Solution
1. Open Azure ML Workspace.
2. Select Prompt Flows.
3. Create flow components in the designer.
4. Connect components using the generated DAG.
5. Add Python scripts and prompts.
6. Deploy as a containerized endpoint.
7. Evaluate quality using built-in evaluators.
8. Optimize and iterate.

### Connect Foundry Resources
1. Open Connections.
2. Create a connection.
3. Connect a Microsoft Foundry resource.
4. Configure authentication.
5. Deploy models from the Model Catalog.

## Evaluation Metrics Mentioned
- Groundedness
- Q&A Quality
- Fluency
- Relevance
- Content Safety
- Custom LLM-as-a-Judge Evaluations

## Code Blocks
No code examples were included in the transcript.

## Actionable Takeaways
- Learn prompt engineering and basic Python.
- Use Prompt Flows for reusable AI microservices.
- Favor serverless inferencing for variable workloads.
- Add automated evaluation early in development.
- Connect Azure ML to Microsoft Foundry to unlock model deployment capabilities.

## Backlink Suggestions
- [[Azure Machine Learning]]
- [[Prompt Engineering]]
- [[Microsoft Foundry]]
- [[Model Deployment]]
- [[GenAIOps]]
- [[AI Evaluation]]

## Tags
#AI300 #AzureML #GenAI #PromptFlow #Foundry #ModelCatalog #GenAIOps
