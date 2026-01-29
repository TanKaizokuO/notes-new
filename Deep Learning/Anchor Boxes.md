---
tags:
  - extension
---

---
## Problem

A basic grid cell can usually detect only **one** object.
*Question*: What happens if a person is standing in front of a car, and **both** their centers fall into the **same** grid cell?

---

## Solution: Anchor Boxes
Predefine a set of shapes (Anchor Boxes) with different aspect ratios (e.g., one tall/thin for pedestrians, one wide for cars). Each grid cell will now predict **multiple** bounding boxes, one for each anchor shape.

![[Pasted image 20260116201908.png]]

---

## Assignment Rule
Each object in the training image is assigned to:
1.  The grid cell containing the object's **center**.
2.  The **Anchor Box** with the highest **[[Intersection over Union (IoU)|IoU]]** with the object's ground truth shape.

---

## Output Size
- **Without Anchors**: Output is $S \times S \times (5 + \text{classes})$
- **With 2 Anchors**: Output is $S \times S \times 2 \times (5 + \text{classes})$