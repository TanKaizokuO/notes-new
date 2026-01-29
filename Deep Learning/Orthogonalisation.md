---
tags:
  - main
---
---

**Orthogonalisation** means solving ML problems in *separate, independent steps*, so one issue does not interfere with another.

---

## Orthogonal Approach (Ideal Pipeline)

1. **Fit training set** well on cost function.
2. **Fit dev set** well on cost function.
3. **Fit test set** well on cost function.
4. Finally, perform well in the **real world**.

Each step should be debugged independently.

---

## Single Number Evaluation Metric

When comparing many models, pick a single number metric to make ranking easier instead of using several [[Evaluation Metrics]].

**Example**: Trained classifier to identify class **X**.
✅ Use **F1 score** (better than accuracy when class imbalance exists).