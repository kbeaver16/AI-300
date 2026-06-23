# Introduction to Retrieval Augmented Generation (RAG)

## Summary
RAG combines retrieval, prompt augmentation, and generation to ground LLM responses in enterprise data.

## Key Concepts
- RAG
- Hallucination Prevention
- Vector Embeddings
- Similarity Search
- Augmented Prompt
- Azure AI Search

## Definitions
### RAG
Architecture that retrieves external knowledge before generation.
### Vector Embedding
Numerical representation of content.
### Augmented Prompt
User query plus retrieved context.

## Examples
- Enterprise chatbot over internal documents.
- Travel document search assistant.

## Step-by-Step Process
1. Create vector index.
2. Convert documents into embeddings.
3. Convert query into embedding.
4. Run similarity search.
5. Retrieve top documents.
6. Build augmented prompt.
7. Generate grounded response.

## Code Blocks
No code provided.

## Actionable Takeaways
- Prefer RAG over frequent fine-tuning for changing knowledge.
- Ground responses using enterprise content.

## Backlinks
[[Azure AI Search]] [[Vector Embeddings]] [[Prompt Flow]] [[RAG]]

## Tags
#AI300 #RAG #Embeddings #AzureAI