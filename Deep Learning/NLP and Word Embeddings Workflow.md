---
tags:
  - extension
---
---

# NLP and Word Embeddings Workflow

## Workflow Steps
1.  **Pre-training**: Learn embeddings on a massive text corpus (1–100B words) or download pretrained ones (Word2Vec, GloVe).
2.  **Transfer**: Initialize your model with these embeddings.
3.  **Fine-tuning**: Adjust embeddings *only* if you have a large labeled dataset.

---

## Visualizing Embeddings: t-SNE
High-dimensional vectors (300D) are impossible for humans to see. We use **t-SNE (t-Distributed Stochastic Neighbor Embedding)** to compress them into 2D or 3D.



* **Goal**: Keep similar words close in the visual map.
* **Warning**: t-SNE is non-linear. Distances in the plot do not perfectly reflect the original vector math (e.g., analogies might look distorted).

---

