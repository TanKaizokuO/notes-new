---
created: 2026-02-05
tags:
  - main
---
---
# [[Retrieval Augmented Generation|RAG]]: Trade-offs & Evaluation Strategy

## 1. Pros & Cons of RAG

### ✅ Pros
* **Rapid Time to Market:** Easier to implement than fine-tuning a model from scratch.
* **Vast Expertise:** Allows the LLM to access massive external datasets without retraining.
* **Context Optimization:**
    * **Saves Context Length:** You don't need to stuff the entire knowledge base into the prompt.
    * **Focus:** Avoids distracting the model with irrelevant context (reduces noise).

### ❌ Cons
* **Trial & Error:** Requires significant tuning of chunk sizes, overlap, and retrieval parameters (k).
* **Reliability:** Can lead to unexpected "I don't know" responses if the retrieval step fails to find the right chunk, even if the answer exists in the database.

---

## 2. Evaluation Framework

To move beyond "trial and error," a formal evaluation pipeline is required.

### A. The Test Set (Ground Truth)
You cannot evaluate without a curated "Golden Dataset." This must include:
1.  **Example Questions:** Real queries users might ask.
2.  **Identified Context:** The specific document chunk IDs that *should* be retrieved.
3.  **Reference Answers:** The ideal human-written response.

### B. Measuring Retrieval (The "R")
How good is the system at finding the right document?

* **Metrics:**
    * **MRR (Mean Reciprocal Rank):** How high up the list is the first relevant result?
    * **nDCG (Normalized Discounted Cumulative Gain):** Measures ranking quality.
    * **Precision@K:** What percentage of the top K results are relevant?
    * **Recall@K:** (Crucial) Did we retrieve the relevant chunk *at all* within the top K results?

> [!TIP] The Priority Metric
> **Recall@K** is often the most important metric here. If the relevant document isn't in the context window (Recall failure), the LLM has zero chance of answering correctly.

* **Keyword Coverage:** analyzing if specific domain keywords from the query appear in the retrieved chunks.

### C. Measuring Generation (The "G")
How good is the system at synthesizing the answer?

* **LLM-as-a-Judge:** Use a stronger model (e.g., GPT-4 or Gemini 1.5 Pro) to score the RAG system's output against the Reference Answer.
* **Scoring Criteria:**
    * **Accuracy:** Is the information factually correct?
    * **Completeness:** Did it miss any key details?
    * **Relevance:** Did it actually answer the user's question?

---
