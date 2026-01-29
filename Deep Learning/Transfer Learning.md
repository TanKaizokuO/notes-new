---
tags:
  - main
---
---

Transfer learning is a [[DL Strategy]] that involves taking a model trained on Task A and adapting it to Task B.

---
![[IMG_0986.png]]
## Core Idea
1. Train a deep network on **Task A** (Large Dataset, e.g., ImageNet).
2. Delete the **last layer** (output layer).
3. Add a **new output layer** for **Task B** (randomly initialized weights).
4. Train on Task B data.
    - **Small Task B data**: Retrain *only* the new last layer (freeze earlier layers).
    - **Large Task B data**: Retrain (fine-tune) the *entire* network.

**Intuition**:
- Task A (Image Recognition) learns edges, textures, and shapes.
- Task B (Radiology) reuses these low-level visual features.

---

## When Transfer Learning is Useful

1. **Task A** has **a lot of data**.
2. **Task B** has **less data**.
3. Low-level features from A are helpful for B.
4. Input types are the same (e.g., both are images, or both are audio).