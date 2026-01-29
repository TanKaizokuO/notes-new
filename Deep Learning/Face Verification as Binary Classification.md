---
tags:
  - extension
---

---

Face verification can alternatively be solved as a binary classification problem.
![[Pasted image 20260117201635.png]]
## Procedure
1.  **Compute Embeddings**: A Siamese network computes $f(x_1)$ and $f(x_2)$.
2.  **Classify**: A classifier (e.g., Logistic Regression or a small Neural Network) takes the embeddings as input and predicts:
    * $1$ if same person
    * $0$ if different person

> [!TIP] Implementation Tip
> Precompute embeddings for database images to significantly speed up verification and recognition tasks.
]]
