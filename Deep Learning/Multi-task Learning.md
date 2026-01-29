---
tags:
  - main
---

---

Multi-task learning  is a [[DL Strategy]] in which a single model learns multiple tasks simultaneously using shared representations.

---
![[Selection_006.png]]
## Example: Autonomous Driving

A single image input needs to predict:
1.  Pedestrian?
2.  Other vehicles?
3.  Traffic signs?
4.  Traffic lights?

**Output Vector**:
$$y^{(i)} \in \mathbb{R}^{4 \times 1}$$

**Dataset Matrix**:
$$Y \in \mathbb{R}^{4 \times m}$$

---

## Loss Function (Multi-task)

Unlike Softmax, we sum the loss across all individual tasks:

$$J = \frac{1}{m}\sum_{i=1}^{m}\sum_{j=1}^{4} L(\hat{y}^{(i)}_j, y^{(i)}_j)$$

*Where $L$ is usually logistic loss (binary cross-entropy) for each component.*

---

## Comparison vs Softmax

* **Softmax**: One example belongs to exactly **one class** (Mutually exclusive).
* **Multi-task**: One example can have **multiple labels** (e.g., a car AND a sign in the same image).

> **Note**: If some task labels are missing (undefined) for certain examples, the summation in the loss function simply skips those terms.

---

## When to use Multi-task Learning?

1.  Tasks share similar **lower-level features**.
2.  Amount of data for each task is roughly similar.
3.  A sufficiently large Neural Network is used (a small NN might underperform compared to separate models).