# RAG vs Structured RAG: Hotel Search Comparison

This project compares the performance of two different Retrieval-Augmented Generation (RAG) approaches for hotel search: Traditional RAG and Structured RAG. The comparison uses a dataset of 144 hotels and evaluates both approaches across 89 test queries using multiple performance metrics.

## Project Overview

The goal is to determine which RAG approach provides better search results for hotel recommendations. We compare:

1. **Traditional RAG**: Pure vector similarity search using OpenAI embeddings
2. **Structured RAG**: Hybrid approach combining metadata filtering with dual vector embeddings

## Dataset

- **Hotels**: 144 hotel records with rich metadata (location, type, amenities, atmosphere, pricing)
- **Test Queries**: 89 diverse search queries with varying complexity
- **Embeddings**: Generated using OpenAI's text-embedding-3-small model (1536 dimensions)

## Methodology

### Traditional RAG
- Creates embeddings for hotel descriptions
- Performs vector similarity search using cosine distance
- Returns top-k most similar hotels based on semantic similarity

### Structured RAG
- Parses queries into structured components (location, hotel type, amenities, etc.)
- Applies metadata filtering to narrow down candidates
- Uses dual embeddings: description + structured data serialization
- Falls back to traditional RAG if no metadata matches found

### Evaluation Metrics
- **Precision**: Relevance of retrieved results
- **Recall**: Coverage of relevant results
- **F1-Score**: Harmonic mean of precision and recall
- **MRR (Mean Reciprocal Rank)**: Quality of ranking
- **Weighted MRR**: MRR weighted by query complexity
- **NDCG@5**: Normalized Discounted Cumulative Gain at rank 5
- **Response Time**: Average query processing time

## Results

| Metric                    | Traditional RAG | Structured RAG | Winner         |
|---------------------------|-----------------|----------------|----------------|
| **Precision**             | 0.2728          | 0.4303         | Structured RAG |
| **Recall**                | 0.8288          | 0.8786         | Structured RAG |
| **F1-Score**              | 0.3655          | 0.4993         | Structured RAG |
| **MRR (Traditional)**     | 0.8331          | 0.8730         | Structured RAG |
| **Weighted MRR**          | 0.7074          | 0.7228         | Structured RAG |
| **NDCG@5**                | 0.7845          | 0.8280         | Structured RAG |
| **Avg Response Time (s)** | 0.2635          | 1.7357         | Traditional RAG |

## Key Findings

### Structured RAG: Point Increases
- **Precision:** +0.1575 (from 0.2728 to 0.4303)
- **F1-Score:** +0.1338 (from 0.3655 to 0.4993)
- **NDCG@5:** +0.0435 (from 0.7845 to 0.8280)
- **Recall:** +0.0498 (from 0.8288 to 0.8786)

### Traditional RAG Advantages:
- **6.6x faster** response time (0.26s vs 1.74s)
- Simpler implementation and maintenance
- Lower computational overhead

### Overall Winner: **Structured RAG**
Structured RAG wins in 5 out of 7 metrics, showing significantly better relevance and ranking quality, making it the superior choice for applications where result quality is prioritized over speed.

## Technical Implementation

### Architecture
- **Vector Database**: Qdrant for efficient similarity search
- **Embeddings**: OpenAI text-embedding-3-small
- **Query Parsing**: GPT-4o-mini for structured query extraction


## Files Structure

```
├── RAG_vs_Structured_RAG.ipynb    # Main notebook with complete implementation
├── Dataset.json                   # Hotel dataset (144 hotels)
├── queries.json                   # Test queries (89 queries)
├── hotels_with_embeddings.json    # Hotels with pre-computed embeddings
├── traditional_rag_results.json   # Traditional RAG evaluation results
├── structured_rag_results.json    # Structured RAG evaluation results
└── README.md                      # This documentation
```

## Conclusion

This comparison demonstrates that **Structured RAG significantly outperforms Traditional RAG** in terms of result quality, with substantial improvements in precision, recall, and ranking metrics. While Traditional RAG maintains a speed advantage, Structured RAG provides a better user experience through more relevant and accurate hotel recommendations.

The hybrid approach of combining semantic search with structured metadata filtering proves to be highly effective for domain-specific search applications where users often have specific requirements (location, amenities, hotel type, etc.).
