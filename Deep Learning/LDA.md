---
tags:
  - main
---
---

# LDA (Linear Discriminant Analysis)

## What is LDA?
LDA is a **dimensionality reduction technique for supervised classification**.
It projects high-dimensional data onto lower dimensions **while maximizing class separability**.

> [!INFO] Definition
> LDA seeks a linear combination of features that characterizes or separates two or more classes of objects or events.

---

## Key Intuition
When 2 classes cannot be separated cleanly in the original 2D space:
1.  LDA finds a **new axis**.
2.  Projects points onto it.
3.  Increases separability (Reduces problem from 2D $\to$ 1D).



---

## Two Important Criteria (Objective)
LDA chooses a projection axis to satisfy two competing goals simultaneously:

> [!GOAL] The LDA Objective
> 1.  **Maximize distance between class means** (Push classes apart).
> 2.  **Minimize variation (scatter) within each class** (Group class members tightly).

This produces maximum discrimination on the new axis.

---

## LDA Steps
1.  **Compute Means**: Calculate the mean vector for each class.
2.  **Compute Covariance**: Calculate the covariance matrix per class.
3.  **Compute Within-Class Scatter ($S_w$):**
    $$S_w = S_1 + S_2$$
4.  **Compute Between-Class Scatter ($S_B$):**
    $$S_B = (\mu_1 - \mu_2)(\mu_1 - \mu_2)^T$$
5.  **Solve Eigenvalues/Eigenvectors:**
    Find the eigenvectors of the matrix:
    $$S_w^{-1}S_B$$
6.  **Sort & Select**: Sort eigenvalues and pick the top $k$.
7.  **Form Projection**: Use corresponding eigenvectors to create the transformation matrix.
8.  **Project**: Transform the original data onto the new LDA axis.

---

## Fisher Discriminant Analysis (FDA)
* **FDA**: Originally formulated for binary classification, finding a **single** discriminant function maximizing the ratio:
    $$J(w) = \frac{(m_2 - m_1)^2}{s_1^2 + s_2^2}$$
    *(Ratio of "difference of means" to "sum of variances")*
* **Relation**: LDA is the generalization of FDA to multiple classes and multiple discriminants.

---

## LDA vs PCA

| Aspect | LDA | PCA |
| :--- | :--- | :--- |
| **Type** | Supervised | Unsupervised |
| **Uses Labels** | Yes | No |
| **Goal** | Max class separability | Max variance (retain info) |
| **Components** | $\le C-1$ (where $C$ is classes) | $\le d$ (dimensions) |
| **Use Case** | Classification + DR | DR + Visualization |
| **Generative** | Yes (can model class params) | No |

---

## Related Notes
* [[Dimensionality Reduction]]