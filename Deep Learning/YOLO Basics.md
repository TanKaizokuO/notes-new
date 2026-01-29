---
tags:
  - main
---
YOLO is an [[CNN Archeitectures]]

---
## Problem with Sliding Windows
Even the convolutional implementation of sliding windows has a key limitation:

- Bounding boxes are influenced by **window position + stride**
- The predicted window may **not align perfectly** with the object's real boundaries
- Increasing accuracy requires:
  - smaller stride (→ expensive)
  - more window scales (→ expensive)

YOLO solves this by learning to **directly predict bounding box coordinates**.

---
## YOLO Core Idea

![[Pasted image 20260116200906.png]]

1. Divide the image into an **$S \times S$ grid**
2. Each grid cell predicts objects **whose center lies inside the cell**
3. Each cell outputs bounding box + class predictions (and objectness score)

---

## Responsibility Rule
> **Rule:** If an object’s center point $(x, y)$ falls into grid cell $i$, then **only grid cell $i$** is responsible for detecting that object.

---

## Output Vector (Per Grid Cell)
Each grid cell outputs a vector:

$$
y = [P_c, b_x, b_y, b_w, b_h, c_1, c_2, \dots, c_k]
$$

Where:
- $P_c$: probability that an object exists in that cell (**objectness**)
- $b_x, b_y$: bounding box center coordinates (relative to the grid cell)
- $b_w, b_h$: bounding box width & height (relative to image/cell)
- $c_1 \dots c_k$: class probabilities

---

## Key Takeaway
✅ YOLO reframes detection as a single regression + classification task  
✅ Predicts bounding boxes directly (not limited by stride or window boundary)  
✅ Enables real-time object detection.

---

The Detailed algorithm is here : [[YOLO Algorithm]]
