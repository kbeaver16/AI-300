# Search Optimization Strategies

## Summary
Explains search algorithms, vector arrangements, and hybrid retrieval techniques used to improve RAG pipelines.

## Key Concepts
- Vector Search
- Keyword Search
- Hybrid Search
- Semantic Re-ranking
- HNSW
- Cosine Similarity
- K-Nearest Neighbors

## Definitions
### HNSW
Hierarchical Navigable Small World graph used for efficient vector search.

### Vector Search
Search based on embedding similarity.

### Keyword Search
Search based on exact lexical matches.

### Hybrid Search
A combination of vector and keyword search.

### Cosine Similarity
A mathematical approach for measuring similarity between vectors.

## Examples
- Hybrid Search + Semantic Re-ranking
- Top-K document retrieval
- Embedding similarity scoring

## Step-by-Step Retrieval Process
1. Create embeddings.
2. Store vectors in HNSW structure.
3. Convert query into embeddings.
4. Run KNN retrieval.
5. Execute vector search.
6. Execute keyword search.
7. Merge results.
8. Apply semantic re-ranking.
9. Return Top-K documents.
10. Build augmented prompt.

## Code Blocks
Transcript includes Azure AI Search JSON request examples for:
- Vector Search
- Keyword Search
- Hybrid Search
- Semantic Re-ranking

## Actionable Takeaways
- Retrieval quality determines RAG quality.
- Hybrid search provides the best balance.
- Semantic re-ranking should be enabled whenever possible.
- HNSW dramatically improves search performance.
- Optimize Top-K carefully.

## Backlink Suggestions
[[Azure AI Search]]
[[HNSW]]
[[Vector Search]]
[[Semantic Search]]
[[Hybrid Search]]
[[RAG]]

## Tags
#AI300 #AzureAISearch #HybridSearch #SemanticSearch #HNSW #RAG