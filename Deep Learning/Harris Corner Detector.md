---
tags:
  - extension
---
---

# Harris Corner Detector

## What is a Corner?
A corner is defined as a region in an image with **gradients in two significantly different orientations**.

* **Flat Region**: No change in intensity in any direction.
* **Edge**: Strong change in intensity in only one direction (along the gradient).
* **Corner**: Strong change in intensity in all directions.

[Image of Harris Corner Detector flat edge corner visualization]

---

## Autocorrelation (The "Cornerness" Idea)
To detect corners, we quantify how much an image patch changes when shifted by a small amount $(\Delta x, \Delta y)$. We compute the sum of squared differences (SSD) between the original and the shifted window.

### 1. Gradient Approximation
First, we compute the image gradients $I_x$ and $I_y$ (derivatives of Intensity $I$). A common discrete approximation is the central difference:

$$
I_x \approx \frac{1}{2}[I(x+1,y)-I(x-1,y)]
$$
$$
I_y \approx \frac{1}{2}[I(x,y+1)-I(x,y-1)]
$$

### 2. The Structure Tensor (Matrix A)
Using Taylor expansion, the SSD for a small shift is approximated using a symmetric matrix $A$ (often called $M$ or the Structure Tensor):

$$
A = \sum_{u,v} w(u,v) \begin{bmatrix} I_x^2 & I_x I_y \\ I_x I_y & I_y^2 \end{bmatrix}
$$

where $w(u,v)$ is a window function (e.g., a Box filter or Gaussian window).

### 3. Eigenvalue Interpretation
The eigenvalues $\lambda_1, \lambda_2$ of matrix $A$ characterize the local geometry of the image patch:

> [!INFO] Interpreting Eigenvalues
> * $\lambda_1 \approx 0$ and $\lambda_2 \approx 0 \implies$ **Flat Region** (No features)
> * $\lambda_1 \gg 0$ and $\lambda_2 \approx 0 \implies$ **Edge** (One dominant direction)
> * $\lambda_1 \gg 0$ and $\lambda_2 \gg 0 \implies$ **Corner** (Two dominant directions)

---

## Harris Response Function
Computing eigenvalues explicitly is computationally expensive (requires square roots). Harris suggested a response score $M_c$ that avoids this.

$$
M_c = \det(A) - \kappa \, \text{trace}(A)^2
$$

Which is mathematically equivalent to:
$$
M_c = \lambda_1\lambda_2 - \kappa(\lambda_1+\lambda_2)^2
$$

* $\det(A) = \lambda_1 \lambda_2$
* $\text{trace}(A) = \lambda_1 + \lambda_2$
* $\kappa$ is an empirical constant (usually $0.04 - 0.06$).

---

## Algorithm Steps
1.  **Compute Gradients**: Calculate $I_x$ and $I_y$ for every pixel.
2.  **Compute Matrix A**: Calculate products $I_x^2$, $I_y^2$, $I_x I_y$ and sum them over a local window (Gaussian smoothing).
3.  **Compute Response**: Calculate $M_c$ for each pixel.
4.  **Threshold**: Keep pixels where $M_c > \text{threshold}$.
5.  **Non-Maximum Suppression**: Pick local maxima to find the precise corner point.

---

## Properties

> [!SUCCESS] Invariant
> * **Rotation Invariant** ✅
>     * The eigenvalues of the ellipse do not change when the image is rotated.

> [!FAILURE] Not Invariant
> * **Scale Invariant** ❌
>     * A sharp corner looks like a curve (edge) when zoomed in too far.

---

**Backlinks:** [[Image Features]]