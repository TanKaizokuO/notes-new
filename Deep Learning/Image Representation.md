---
tags:
  - main
---
---

# Digital Image Representation & Filtering

## Digital Image as a Pixel Matrix
A **digital image** is represented as a **pixel matrix**:

> [!INFO] Definition: Pixel
> **Pixel** = The smallest unit of a digital image.

* **Pixel Values:**
    * **0 to 255** (8-bit depth): $0 = \text{black}$, $255 = \text{white}$
    * Sometimes normalized to range **0 to 1**
* **Channels:**
    * **Grayscale image**: Single matrix (1 channel)
    * **Color image (RGB)**: 3 matrices (Red, Green, Blue channels)

**Resolution & Geometry:**
* **Resolution**: Image detail (measured in PPI / DPI). Depends on sensor capture.
* **Aspect ratio**: The ratio of $\text{width} : \text{height}$.

---

## Pixel Value vs. Intensity
It is important to distinguish between the stored value and the physical interpretation.

* **Pixel value**: The stored numeric value at a specific pixel location.
* **Intensity**: The perceived brightness or light energy at that pixel.

### Grayscale Case
The pixel value directly represents intensity.
* *Example (8-bit):* $0 \to \text{black}$, $255 \to \text{white}$

### Color Case
Intensity is derived from the combination of RGB channels, often computed as a weighted sum to match human perception:

$$I = w_R R + w_G G + w_B B$$

---

## Color Space Representations
While **RGB** is the most common for displays, other spaces exist for specific use cases.

* **CMYK**: Used for printers (Subtractive color mixing).
* **Other Spaces**:
    * XYZ, YUV, Lab, YCbCr, HSV
* **Standardization**:
    * **CIE** (Commission Internationale de l'Éclairage) standardizes these color spaces.

---

## Image as a Function
Mathematically, an image can be viewed as a function:

$$f: \mathbb{R}^2 \to \mathbb{R}$$

Where:
* **Input**: Coordinates $(x, y)$
* **Output**: Intensity value (e.g., $0 \dots 255$)

*Note: This mathematical view allows us to apply standard calculus and signal processing operations to images.*

---

## Image Transformations
Basic operations modify the function $I(x,y)$.

### 1. Brightness / Lightening
Adding a constant bias to the image intensity.
$$I'(x,y) = I(x,y) + \text{value}$$
*Constraint:* $0 \le \text{value} \le 255$ (must be clamped).

### 2. Mirroring (Reflection)
Mirroring about the vertical $y$-axis:
$$I'(x,y) = I(-x, y)$$

---

## Image as a Signal
A **signal** is any physical quantity varying with time or space. Images carry information, therefore:
* Image = **2D Signal**
* A pixel can be represented as a function, for example:
    $$f(x,y) = 10x + 2y$$

### Image in Frequency Domain
Since an image is a 2D signal, it can be transformed using the **Fourier Transform**:
1.  Converts **Spatial Domain** $\to$ **Frequency Domain**.
2.  Decomposes image into constituent sine/cosine waves.
3.  Used to analyze frequency content (e.g., high frequency = edges/noise, low frequency = smooth areas).

---

# Image Filtering

## Image Filters (Local Operation)
Filters modify pixel values based on the values of their surrounding neighbors.
* *Example:* A moving average computes the mean of the neighborhood.

### Linear Filter
A **linear filter** replaces each pixel with a weighted sum of its neighbors. The weight matrix is known variously as a:
* **Kernel**
* **Mask**
* **Filter**

## Moving Average Filter
A common smoothing filter (Low-pass filter):
* Averages neighborhood pixels.
* **Effect**: Reduces noise, but blurs edges.

## Edge Filters
Edge filters extract high-frequency information. They exaggerate pixels where the intensity difference is significant.
* **Goal**: Detect vertical, horizontal, or diagonal edges.
* **Common Operators**:
    * Prewitt
    * Sobel
    * Roberts

---
## Convolution
**Convolution** is the core mathematical operation for linear filtering:
1.  Slide the kernel over the image.
2.  Compute the sum of products (weighted sum) at each position.
3.  **Result**: Highlights specific features such as edges, corners, or textures depending on the kernel used.
---

> [!NOTE] Related Note
> See: [[Image Features]]