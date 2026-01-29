---
tags:
  - extension
---
---
### For a case of classifier of class X
## Precision

**Of the samples predicted as X, how many are actually X?**

$$Precision = \frac{TP}{TP + FP}$$

---

## Recall

**What % of actual X is correctly classified as X?**

$$Recall = \frac{TP}{TP + FN}$$

---

## Accuracy

**% of correct predictions overall.**

$$Accuracy = \frac{TP + TN}{TP + TN + FP + FN}$$

---

## F1 Score

F1 score is the **harmonic mean** of precision and recall.

$$F1 = \frac{2 \cdot Precision \cdot Recall}{Precision + Recall}$$

**Used when:**
- Class imbalance exists.
- We want a balance of precision + recall.