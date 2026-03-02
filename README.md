# semantic-search 🧠 — Azure AI hybrid search — HNSW vectors + keyword, 1536-dim embeddings.

<p align="center">
  <img src="assets/hero.png" alt="Semantic Search hero" width="1100">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white" alt="TypeScript">
  <img src="https://img.shields.io/badge/Azure_AI_Search-0078D4?style=for-the-badge" alt="Azure AI Search">
  <img src="https://img.shields.io/badge/OpenAI-412991?style=for-the-badge" alt="OpenAI">
  <img src="https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white" alt="Node.js">
</p>

A hybrid search system combining HNSW vector similarity (1536-dim OpenAI embeddings) with BM25 keyword search via Azure AI Search. Powers natural language queries across job postings, contacts, and notes with relevance ranking that beats pure vector or pure keyword approaches.

> **This is a closed-source project. The README documents the architecture and learnings.**

## What it does

- Hybrid search: HNSW vector similarity + BM25 keyword matching
- 1536-dimensional OpenAI embeddings for semantic understanding
- Searches across job postings, contacts, and personal notes
- Relevance ranking combines vector score and keyword score
- Supports filters: date range, company, role level, location
- Sub-100ms query latency on 94K+ indexed documents

## How it works

```
Indexing:
  Document ingested
    → Generate 1536-dim embedding via OpenAI
    → Index in Azure AI Search
      (HNSW for vectors, inverted index for keywords)

Querying:
  Search query
    → Embed query via OpenAI
    → Hybrid retrieval (vector + keyword)
    → Reciprocal rank fusion
    → Apply filters
    → Return ranked results
```

## Tech stack

- **Search:** Azure AI Search (hybrid mode)
- **Embeddings:** OpenAI text-embedding-ada-002 (1536 dimensions)
- **Index:** HNSW for vector, BM25 for keyword
- **Runtime:** Node.js
- **Database:** PostgreSQL as source of truth, Azure AI Search as index

## What I learned

- Hybrid search (vector + keyword) consistently outperforms either approach alone -- "senior PM at fintech" needs both semantic understanding and keyword precision
- HNSW index parameters matter: `efConstruction=400` and `m=4` gave the best latency/recall tradeoff for our dataset size
- Embedding the query at search time adds ~50ms -- caching frequent queries cut P95 latency by 40%
