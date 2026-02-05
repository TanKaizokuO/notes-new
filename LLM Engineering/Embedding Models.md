---
created: 2026-02-05
tags:
  - extension
---
---
### Embedding Models Landscape: Evolution & Comparison

An embedding model converts text into a mathematical vector list (e.g., `[0.1, -0.5, 0.9...]`). The choice of model determines the quality of semantic search in RAG applications.

---

## 1. The Early Era (Static Embeddings)

### Word2Vec (2013)
*Created by Google.* The breakthrough that showed vector math could represent meaning (e.g., King - Man + Woman = Queen).

* **Type:** Static (Context-independent).
* **Dimensions:** Typically 300.
* **Pros:**
    * ⚡ **Extremely Fast:** Very low computational cost.
    * 🟢 **Simple:** Easy to understand and implement.
* **Cons:**
    * 🔴 **No Context:** The word "Bank" has the exact same vector in "River bank" and "Bank deposit." This is fatal for modern semantic search.
    * 🔴 **Out of Vocabulary (OOV):** Struggles with words it hasn't seen before.

---

## 2. The Transformer Era (Contextual Embeddings)

### BERT (2018)
*Bidirectional Encoder Representations from Transformers (Google).* Changed NLP by reading text in both directions to understand context.

* **Type:** Contextual.
* **Dimensions:** 768 (Base) or 1024 (Large).
* **Pros:**
    * 🟢 **Context Aware:** "Bank" (river) and "Bank" (money) finally have different vectors.
    * 🟢 **Deep Understanding:** Captures complex sentence structures better than Word2Vec.
* **Cons:**
    * 🔴 **Heavy:** Requires significantly more compute (GPU recommended).
    * 🔴 **Not optimized for Search out-of-box:** Raw BERT embeddings are actually poor for semantic similarity without fine-tuning (led to the rise of SBERT).

---

## 3. The Modern Era: Local / Open Source

### all-MiniLM-L6-v2 (Sentence-Transformers)
The standard "Hello World" model for local RAG and python tutorials.

* **Type:** Sentence Transformer (HuggingFace).
* **Dimensions:** 384.
* **Max Sequence:** 256 tokens.
* **Pros:**
    * ⚡ **Blazing Fast:** Can run on a standard CPU comfortably.
    * 🆓 **Free & Private:** Runs entirely locally; no data leaves your server.
    * 🟢 **Efficient Storage:** Small dimension (384) means vector databases (Chroma/Pinecone) stay small.
* **Cons:**
    * 🔴 **Limited Context:** Only accepts 256 tokens; truncates long texts.
    * 🔴 **Lower Accuracy:** Not as nuanced as the massive OpenAI/Gemini models.

---

## 4. The Modern Era: Proprietary APIs (SOTA)

### OpenAI `text-embedding-3-small`
The standard choice for most commercial RAG applications due to cost-efficiency.

* **Type:** API (SaaS).
* **Dimensions:** 1536 (can be shortened).
* **Max Input:** 8,191 tokens.
* **Pros:**
    * 🟢 **Cost Effective:** Significantly cheaper than the previous `ada-002` model.
    * 🟢 **High Performance:** Scores very high on MTEB (Massive Text Embedding Benchmark).
    * 🟢 **Flexible:** Supports shortening embeddings (dimensionality reduction) without retraining.
* **Cons:**
    * 🔴 **Data Privacy:** Data must be sent to OpenAI servers.
    * 🔴 **Black Box:** We don't know the training data or architecture.

### OpenAI `text-embedding-3-large`
The "heavy hitter" for when accuracy matters more than cost or latency.

* **Type:** API (SaaS).
* **Dimensions:** 3072.
* **Max Input:** 8,191 tokens.
* **Pros:**
    * 🟢 **SOTA Accuracy:** One of the best performing embedding models in existence.
    * 🟢 **Nuance:** Better at distinguishing subtle differences in very similar documents.
* **Cons:**
    * 🔴 **Expensive:** Higher API costs.
    * 🔴 **Storage Heavy:** 3072 dimensions require 2x the storage space in your Vector DB compared to `small`.

### Google Gemini `text-embedding-004` (formerly 001)
Google's answer to OpenAI, integrated deeply into the Vertex AI ecosystem.

* **Type:** API (SaaS).
* **Dimensions:** 768 output (optimised for performance/storage balance).
* **Pros:**
    * 🟢 **Task Types:** Allows you to specify the "task type" (e.g., `RETRIEVAL_DOCUMENT`, `SEMANTIC_SIMILARITY`) to optimise the vector for the specific use case.
    * 🟢 **Integration:** Seamless if you are already in the Google Cloud/Vertex ecosystem.
    * 🟢 **Performance:** Extremely competitive on benchmarks, often outperforming OpenAI on retrieval tasks specifically.
* **Cons:**
    * 🔴 **Rate Limits:** Can be stricter on free/starter tiers compared to OpenAI.
    * 🔴 **Latency:** Occasionally higher latency than `text-embedding-3-small`.

---

## Summary Comparison Table

| Model | Hosted | Dimensions | Best Use Case |
| :--- | :--- | :--- | :--- |
| **Word2Vec** | Local | 300 | Legacy systems, single word analysis |
| **BERT** | Local | 768 | NLP Research, Classification tasks |
| **all-MiniLM** | Local | 384 | **Local RAG**, POCs, Privacy-first apps |
| **OpenAI 3-Small** | API | 1536 | **General Production RAG** (Best Balance) |
| **OpenAI 3-Large** | API | 3072 | High-stakes retrieval, Legal/Medical RAG |
| **Gemini Emb-004**| API | 768 | Google Cloud users, Task-specific retrieval |

> [!TIP] Recommendation
> Start with **all-MiniLM-L6-v2** to build your pipeline essentially for free. 
> Switch to **OpenAI text-embedding-3-small** or **Gemini** only when you hit accuracy limits or need to process very long documents (chunk sizes > 256 tokens).

---
Related : [[Retrieval Augmented Generation|RAG]]

