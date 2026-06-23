# Chunking Strategies for RAG

## Summary
Compares multiple chunking approaches used when preparing documents for vector indexing and retrieval.

## Key Concepts
- Fixed-Size Chunking
- Semantic Chunking
- Recursive Chunking
- Adaptive Chunking
- Context-Enriched Chunking
- AI-Driven Chunking

## Definitions
### Chunking
The process of splitting large documents into smaller units before indexing.

### Semantic Chunking
Chunking based on logical boundaries and meaning.

### Recursive Chunking
Hierarchical splitting using sections, paragraphs, and sentences.

### Adaptive Chunking
Dynamic chunk sizing based on content complexity.

## Examples
### Fixed Size
- 2,000 characters
- 500-character overlap

### Context-Enriched
- Chunk text
- Add keywords
- Add summaries
- Store metadata

## Step-by-Step Process
1. Examine source documents.
2. Choose chunking method.
3. Determine chunk size.
4. Configure overlap.
5. Add metadata if needed.
6. Generate embeddings.
7. Store vectors.
8. Evaluate retrieval quality.

## Code Blocks
No code examples were included.

## Actionable Takeaways
- Semantic chunking often improves retrieval relevance.
- Overlaps help preserve context.
- Metadata can improve retrieval quality.
- Recursive chunking provides fine-grained control.
- AI-driven chunking offers flexibility but increases cost.

## Backlink Suggestions
[[RAG]]
[[Embeddings]]
[[Vector Search]]
[[Semantic Search]]
[[Azure AI Search]]

## Tags
#AI300 #Chunking #RAG #Embeddings #VectorSearch