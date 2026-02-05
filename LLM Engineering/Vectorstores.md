---
created: 2026-02-05
tags:
  - extension
---
---
### RAG Architecture: Embeddings & Vector Stores

This note covers the two critical components of the Retrieval Augmented Generation pipeline:
1.  **Embedding Models :** Converting text to numbers.
2.  **Vector Databases:** Storing and searching those numbers.

---

### Vector Databases

Once you have vectors, you need a place to store them. The market is split between **Specialized (Native)** vector DBs and **Integrated (General Purpose)** DBs.

### Category A: Open Source / Specialized
*Built from the ground up specifically for vector math.*

| Database   | Description & Pros                                                                                                                                                  | Cons                                                                                      |
| :--------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :---------------------------------------------------------------------------------------- |
| **Chroma** | **The Developer Favorite.** <br>✅ Python-native (pip install chroma). <br>✅ Runs in-memory or persists to disk easily. <br>✅ Perfect for prototyping and Notebooks. | ❌ Not designed for massive scale (millions of vectors) in the local version.              |
| **Qdrant** | **The Performance King.** <br>✅ Written in Rust (extremely fast). <br>✅ Great filtering capabilities. <br>✅ Has a cloud tier and a local docker container.          | ❌ Slightly steeper learning curve than Chroma for setup.                                  |
| **FAISS**  | **The Raw Engine (Meta).** <br>✅ Not a database, but a *library*. <br>✅ The gold standard for speed/indexing algorithms (HNSW).                                     | ❌ No "Database" features (no CRUD, no metadata storage—you have to manage that yourself). |

### Category B: Mainstream Databases
*Traditional databases that added Vector Search capabilities.*

| Database     | Description & Pros                                                                                                                                                | Cons                                                                                  |
| :----------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------ |
| **Postgres** | **(via `pgvector`).** <br>✅ "Boring" tech: ACID compliance, backups, and reliability you already trust. <br>✅ Keep your app data and vectors in the *same* place. | ❌ Slower than Qdrant/Milvus at massive scale (>10M vectors) without tuning.           |
| **MongoDB**  | **(Atlas Vector Search).** <br>✅ Excellent for unstructured data (JSON). <br>✅ Natural fit if your metadata is complex/nested.                                    | ❌ Vector search is often tied to their Atlas (Cloud) offering, less flexible locally. |
| **Elastic**  | **(Elasticsearch).** <br>✅ The King of **Hybrid Search** (combining Keyword search + Vector search). <br>✅ Proven scalability.                                    | ❌ Heavy resource usage (Java-based); complex to manage; overkill for simple RAG.      |

---

## Summary Recommendation

> [!TIP] Suggested Stacks
> 
> **For Prototyping / Personal Projects:**
> * **Model:** `all-MiniLM-L6-v2` (Local)
> * **DB:** `Chroma` (Local)
> 
> **For Production (Startups/SaaS):**
> * **Model:** `OpenAI text-embedding-3-small`
> * **DB:** `Postgres (pgvector)` (if you already use SQL) OR `Qdrant` (if you want pure speed).
> 
> **For Enterprise (Complex Data):**
> * **Model:** `Google Gemini` or `OpenAI Large`
> * **DB:** `Elasticsearch` (for Hybrid Search requirements).

---
Related : [[Retrieval Augmented Generation|RAG]]

