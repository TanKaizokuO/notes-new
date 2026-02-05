---
created: 2025-02-05
tags:
  - main
---
---
# RAG: LLM Architectures & Vector Embeddings
## 1. Auto-Regressive vs. Auto-Encoding LLMs

Understanding **Retrieval Augmented Generation (RAG)** requires distinguishing between the two primary architectures used in Large Language Models.

### Auto-Regressive LLMs (Generation)
* **Mechanism:** Predicts the *next* token in a sequence based on previous tokens.
* **Directionality:** Unidirectional (usually left-to-right).
* **Primary Use Case:** Text generation (writing, coding, chatting).
* **Examples:** GPT-4, Llama 3, Claude.
* **Role in RAG:** These are the models that *synthesize* the final answer using the retrieved data.

### Auto-Encoding LLMs (Understanding)
* **Mechanism:** Predicts *masked* tokens within a sequence by looking at context from both sides (bidirectional).
* **Directionality:** Bidirectional (reads the whole sentence at once).
* **Primary Use Case:** calculating **Vector Embeddings**, sentiment analysis, classification.
* **Examples:** **BERT** (Bidirectional Encoder Representations from Transformers) by Google.
* **Role in RAG:** These models convert user queries and documents into vectors to enable semantic searching.

> [!NOTE] Key Distinction
> **Auto-Regressive** models write the answer. **Auto-Encoding** models find the information.

---

## 2. Vector Embeddings

In a RAG system, "meaning" is translated into numbers. Auto-encoding LLMs are typically used to generate these **Vector Embeddings**.

### What is a Vector?
Mathematically, a vector is an array of numbers (coordinates) in a high-dimensional space. In NLP, it is a **semantic representation** of an input.

* **Granularity:** A vector can represent:
    * A character
    * A token
    * A word
    * An entire document
    * Abstract concepts (e.g., "sadness" or "efficiency")

### Semantic Proximity
The core philosophy of embeddings is that **meaning = location**.
* Inputs with similar meanings are located close to each other in vector space.
* Inputs with different meanings are far apart.
* *Example:* The vector for `Apple` will be closer to `Fruit` than to `Car`.

### Vector Mathematics
Because meaning is represented numerically, we can perform mathematical operations on language concepts.

$$\text{Meaning}(\text{Input}) \approx \vec{v} \in \mathbb{R}^n$$

We can use vector arithmetic to manipulate abstract relationships. The classic example is:

$$\vec{v}_{\text{King}} - \vec{v}_{\text{Man}} + \vec{v}_{\text{Woman}} \approx \vec{v}_{\text{Queen}}$$

This allows the system to understand analogies and relationships without explicit programming.

---

## 3. The RAG Connection
1.  **Ingestion:** Documents are passed through an **Auto-Encoding LLM** to create vectors.
2.  **Retrieval:** The user's query is converted to a vector. The system searches for document vectors that are mathematically "closest" (Cosine Similarity).
3.  **Generation:** The relevant text is sent to an **Auto-Regressive LLM** to generate the human-readable response.
---
