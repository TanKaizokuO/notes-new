---
tags:
  - extension
---
---
## Definition
![[Pasted image 20260116201644.png]]

IoU measures the overlap between two bounding boxes (e.g., the **Predicted Box** vs. the **Ground Truth Box**).



$$\text{IoU} = \frac{\text{Area of Intersection}}{\text{Area of Union}}$$

---

## Interpretation
- **IoU = 1**: Perfect overlap.
- **IoU = 0**: No overlap.
- **Threshold**: Typically, if IoU $\ge 0.5$, the detection is considered "correct" (True Positive). Stricter benchmarks use 0.6, 0.7, etc.

---

## Uses
✅ **Evaluation**: Metric for bounding box accuracy.
✅ **NMS**: Used in [[Non-Max Suppression (NMS)|Non-Max Suppression]] to filter overlapping boxes.
✅ **Anchor Assignment**: Used to assign objects to the best matching [[Anchor Boxes|Anchor Box]].