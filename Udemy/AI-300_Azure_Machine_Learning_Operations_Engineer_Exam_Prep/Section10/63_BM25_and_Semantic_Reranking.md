# BM25 Ranking and Semantic Re-ranking

## Summary
Explains how Azure AI Search retrieves, scores, and ranks results using BM25, TF-IDF concepts, weighted fields, and semantic re-ranking.

## Key Concepts
- BM25 Ranking
- TF-IDF
- Inverted Index
- Lexical Analysis
- Semantic Re-ranking
- Search Score
- Re-Ranker Score

## Definitions
### Inverted Index
A mapping of terms to documents where those terms appear.

### BM25
A ranking algorithm that scores documents using term frequency, rarity, and document length.

### TF-IDF
Term Frequency – Inverse Document Frequency, a weighting mechanism for ranking relevance.

### Semantic Re-ranking
An NLP-based process that reorders BM25 results based on semantic meaning.

## Examples
- Query: "What is the capital of France?"
- BM25 may prioritize keyword matches.
- Semantic ranking prioritizes results about Paris.

## Step-by-Step Search Process
1. Ingest documents.
2. Perform lexical analysis.
3. Build inverted index.
4. Process user query.
5. Retrieve candidate documents.
6. Calculate BM25 scores.
7. Apply field weighting.
8. Generate summaries.
9. Run semantic classifier.
10. Produce re-ranked results.

## Code Blocks
No executable code provided; transcript discusses Azure AI Search configuration.

## Actionable Takeaways
- Enable semantic re-ranking for better retrieval quality.
- Configure weighted fields carefully.
- Understand BM25 before tuning search relevance.
- Hybrid + Semantic generally outperforms BM25 alone.

## Backlink Suggestions
[[Azure AI Search]]
[[BM25]]
[[TF-IDF]]
[[Semantic Search]]
[[RAG]]

## Tags
#AI300 #AzureAISearch #BM25 #SemanticSearch #RAG