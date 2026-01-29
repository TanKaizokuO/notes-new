---
tags:
  - extension
---


---
## Problem
A single object might be detected multiple times by adjacent grid cells, resulting in several overlapping bounding boxes for the same object.

---

## Goal
Keep only **one** best bounding box per object and suppress (discard) the others.

---

![[Pasted image 20260116201712.png]]
## Algorithm
1.  Discard all boxes with $P_c \le$ threshold (e.g., 0.6).
2.  **Pick** the box with the highest $P_c$ remaining.
3.  **Suppress** (remove) any other box that has high **[[Intersection over Union (IoU)|IoU]]** (e.g., > 0.5) with the box picked in Step 2.
4.  Repeat steps 2-3 until no boxes remain.

---

## Multi-class Case
If you have multiple classes (Cars, Pedestrians), run NMS **independently** for each class.