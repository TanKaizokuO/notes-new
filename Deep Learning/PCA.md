---
tags:
  - main
---
---
# Principal Component Analysis

## Intuition
PCA is like **taking a picture** of the dataset and keeping as much information as possible:
- choose the **best angle**
- project to fewer dimensions with minimal information loss

Goal: find projection direction such that projected points are **as spread out as possible**.

---

## What PCA Optimizes
PCA finds directions (principal components) that:
- maximize variance in projected data
- decorrelate features (orthogonal axes)

> “PCA captures underlying patterns by identifying directions with maximum variance.”

---

## Ingredients Needed
- mean
- variance
- covariance
- eigenvectors & eigenvalues

---

## Variance
Variance = spread of data along an axis.

Limitation:
- variance measures spread parallel to axes only
- it misses diagonal correlation (when x ↑ and y ↑ together)

---

## Covariance
Covariance measures how two dimensions change together.
- positive covariance → increase together
- negative covariance → one increases while other decreases
- ~0 covariance → little/no linear relationship

---

## Covariance Matrix Σ
For 2D:
$$
\Sigma =
\begin{bmatrix}
Cov(x,x) & Cov(x,y) \\
Cov(y,x) & Cov(y,y)
\end{bmatrix}
$$
Properties:
- symmetric matrix
- diagonal = variances
- off-diagonal = covariances
- captures **shape** of data

---

## Eigenvectors and Eigenvalues
### Eigenvector
A vector whose direction does not change under linear transformation.

### Eigenvalue (λ)
Scalar that describes magnitude scaling along eigenvector direction.

Key PCA meaning:
- eigenvectors = principal directions
- eigenvalues = amount of variance in that direction



---

## PCA Algorithm (Conceptual)
1. Center data (subtract mean)
2. Compute covariance matrix Σ
3. Find eigenvalues + eigenvectors of Σ
4. Sort eigenvectors by eigenvalues (descending)
5. Pick top-k eigenvectors
6. Project data on those eigenvectors

---

## PCA Output
- New coordinate system = principal components
- Reduced dimension while preserving max variance

**See also:** [[PCA - Solved Example]]
