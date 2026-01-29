---
tags:
  - main
---
---
## Overview
**Object Localization** = Classify the object + Draw a bounding box around it.

![[Pasted image 20260116195539.png]]
### Difference from Classification
- **Classification**: What is the object? (e.g., "Car")
- **Localization**: What is the object + **Where** is it? (e.g., "Car" at coordinates $x, y$)

---

## Output Format (Bounding Box)
![[Pasted image 20260116195643.png]]
The bounding box is typically encoded as 4 numbers:

- $b_x, b_y$ = Center of the box
- $b_w, b_h$ = Width & Height of the box

**Model Output Vector:**
$$\hat{y} = [P_c, b_x, b_y, b_w, b_h, c_1, c_2, \dots]$$
*Where $P_c$ is the probability of an object being present.*

---

## Training (Supervised Learning)
You need:
- **Input ($X$)**: The image.
- **Label ($y$)**: Class label + Bounding box coordinates.

---
## Loss Function
![[Pasted image 20260116195715.png]]
The loss is a weighted sum of:
1.  **Classification Loss**: Usually Softmax Cross-Entropy (for the class).
2.  **Regression Loss**: $L1$ or $L2$ (Squared Error) for the bounding box coordinates ($b_x, b_y, b_w, b_h$).

> **Note**: In practice, squared error is common for explanation, but specialized losses (like IoU loss or Log Loss for classification) are often used in production.