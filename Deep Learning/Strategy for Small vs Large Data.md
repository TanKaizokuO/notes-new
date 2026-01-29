---
tags:
  - main
---
---

## The Nature of Computer Vision
Vision problems require mapping pixels to objects, which is a highly complex function. Because of this complexity, even million-image datasets can feel like "small data" relative to the problem difficulty.



---

## The Data vs. Engineering Tradeoff

The approach changes based on data volume:

| **Large Data Regime** | **Small Data Regime** |
| :--- | :--- |
| Simpler architectures work well | Requires more hand-engineering |
| Less feature engineering required | Architecture hacks and tricks are vital |
| "Let the data speak" | Heavy reliance on transfer learning and tricks |

> [!NOTE] The Reality
> Computer vision often lives in the **Small Data Regime**, especially for tasks like **Object Detection** and **Segmentation**, where labeling is expensive.

---

## Sources of Knowledge
Model performance is derived from two sources:
1.  **Labeled Data** $(x, y)$
2.  **Hand-Engineering** (Architecture choice + features + hyperparameters).

**Rule**: As data decreases, dependence on engineering increases.

See also: [[Benchmark Tricks vs Production Reality]]