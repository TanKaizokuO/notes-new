---
tags:
  - extension
---
# Histogram of Oriented Gradients (HoG)

## Main Use
* **Object Detection**: Classically used for pedestrian detection (Dalal–Triggs, 2005).
* **Feature Extraction**: Captures edges and gradient structures (texture), providing a robust representation of shape and contour.
* **Classification**: Typically acts as a feature extractor fed into a classifier (like a Linear SVM).

---

## HoG Pipeline
The HoG descriptor generation follows a specific 5-step process:

1.  **Divide image into small spatial cells** (e.g., $8 \times 8$ pixels).
2.  **Compute gradients** for every pixel in the cell.
3.  **Accumulate orientation histograms** for each cell based on gradient magnitude and direction.
4.  **Normalize** over larger spatial blocks (contrast normalization).
5.  **Concatenate** all normalized histograms into a single 1D feature vector.



---

### Gradient Computation
For every pixel, we compute the gradient vector to understand the direction of intensity change.

**Gradient in X and Y:**
$$G_x = I(x+1, y) - I(x-1, y)$$
$$G_y = I(x, y+1) - I(x, y-1)$$

**Magnitude (Strength of edge):**
$$M = \sqrt{G_x^2 + G_y^2}$$

**Orientation (Direction of edge):**
$$\theta = \arctan\left(\frac{G_y}{G_x}\right)$$

> [!NOTE] Histogram Binning
> Each pixel votes for an orientation bin (e.g., 9 bins for $0^\circ-180^\circ$) weighted by its magnitude $M$.

---

## Normalization (Contrast Normalization)
Gradients are sensitive to overall lighting. To fix this, histograms are normalized across larger groups of cells called **Blocks**.

* **Goal**: Reduce sensitivity to illumination changes and shadows.
* **Method**: Group cells into blocks (e.g., $2 \times 2$ cells), calculate the "energy" of the block, and normalize the cells within it.

---

**Backlinks:** [[Image Features]]