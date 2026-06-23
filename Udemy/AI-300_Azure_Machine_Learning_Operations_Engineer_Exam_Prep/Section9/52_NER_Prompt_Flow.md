# Named Entity Recognition Prompt Flow

## Summary
Build a Prompt Flow microservice that extracts named entities (for example job roles or food items) from user text using an LLM plus a Python post-processing step.

## Key Concepts
- Prompt Flow
- Named Entity Recognition (NER)
- LLM Component
- Python Component
- Managed Identity
- Data Cleansing
- Real-time Endpoints

## Definitions
### NER
Extraction of a specific entity type from text.
### LLM Component
Prompt Flow node that sends prompts to a deployed model.
### Data Cleansing Component
Python node that formats model output into a structured schema.

## Examples
- Job role: "Nikita is a senior software engineer" -> senior software engineer
- Food: "I love eating pizza" -> pizza

## Step-by-Step Process
1. Create Prompt Flow.
2. Grant Storage Blob Data Contributor role.
3. Create inputs: entity_type and text.
4. Add LLM node with extraction prompt.
5. Connect deployed GPT model.
6. Add Python cleansing node.
7. Connect outputs.
8. Test.
9. Deploy endpoint.

## Code Blocks
Python function used to transform comma-separated entities into a list structure.

## Actionable Takeaways
- Structure outputs before exposing APIs.
- Use Prompt Flow DAG for debugging.
- Deploy to managed compute for lower latency.

## Backlinks
[[Prompt Flow]] [[Azure ML]] [[NER]] [[Prompt Engineering]]

## Tags
#AI300 #PromptFlow #NER #AzureML