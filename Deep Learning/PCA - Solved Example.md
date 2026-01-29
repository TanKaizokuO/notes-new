---
tags:
  - extension
---
---

# Solved Example: PCA (2D → 1D)

## Dataset
We start with a dataset consisting of 4 examples with 2 features ($X_1, X_2$).

| Feature | Ex1 | Ex2 | Ex3 | Ex4 |
| :--- | :--- | :--- | :--- | :--- |
| **$X_1$** | 4 | 8 | 13 | 7 |
| **$X_2$** | 11 | 4 | 5 | 14 |

**Task**: Reduce the dimensionality from **2D $\to$ 1D** using Principal Component Analysis (PCA).

---

## Step 1: Compute Mean
First, calculate the mean vector $\bar{X}$ to center the data.

$$\bar{X_1} = \frac{4+8+13+7}{4} = 8$$
$$\bar{X_2} = \frac{11+4+5+14}{4} = 8.5$$

> [!NOTE] Centering
> PCA requires data to be centered around the origin. We will subtract these means from the data points in later steps.

---

## Step 2: Covariance Matrix
Compute the covariance matrix $S$ to understand how features vary with respect to each other.

$$
S =
\begin{bmatrix}
Cov(X_1, X_1) & Cov(X_1, X_2) \\
Cov(X_2, X_1) & Cov(X_2, X_2)
\end{bmatrix}
=
\begin{bmatrix}
14 & -11 \\
-11 & 23
\end{bmatrix}
$$

* $Var(X_1) = 14$
* $Var(X_2) = 23$
* $Cov(X_1, X_2) = -11$ (Negative correlation)

---

## Step 3: Eigenvalues
Solve the characteristic equation to find the magnitude of variance in principal directions.

$$\det(S - \lambda I) = 0$$

**Eigenvalues obtained:**
* $\lambda_1 = 30.3849$ (Dominant variance)
* $\lambda_2 = 6.6151$

---

## Step 4: Eigenvectors
Compute the eigenvectors corresponding to each eigenvalue. These represent the **directions** of the new axes.

**Eigenvector for largest eigenvalue ($\lambda_1$):**
This is the direction of maximum variance (PC1).
$$
e_1 =
\begin{bmatrix}
0.5574 \\
-0.8303
\end{bmatrix}
$$

**Eigenvector for second eigenvalue ($\lambda_2$):**
This is orthogonal to PC1.
$$
e_2 =
\begin{bmatrix}
0.8303 \\
0.5574
\end{bmatrix}
$$



---

## Step 5: Projection (1st Principal Component)
To reduce dimensions to 1D, we project the centered data onto the first eigenvector ($e_1$).

**Formula:**
$$PC_1 = e_1^T (X - \bar{X})$$

**Resulting 1D values for each example:**

| Example | Original $(X_1, X_2)$ | Centered $(X-\bar{X})$ | Projected Value (PC1) |
| :--- | :--- | :--- | :--- |
| **Ex1** | $(4, 11)$ | $(-4, 2.5)$ | **-4.3052** |
| **Ex2** | $(8, 4)$ | $(0, -4.5)$ | **3.7361** |
| **Ex3** | $(13, 5)$ | $(5, -3.5)$ | **5.6928** |
| **Ex4** | $(7, 14)$ | $(-1, 5.5)$ | **-5.1238** |

---

## Interpretation
> [!SUCCESS] Result
> We have successfully compressed the data from 2 coordinates per point to 1 coordinate.
>
> The projection axis $e_1$ captures the **maximum variance** ($\lambda_1 \approx 30.4$), meaning this single number preserves the most information possible about the original spread of the data.

**Backlinks:** [[PCA]]