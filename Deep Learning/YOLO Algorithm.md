---
tags:
  - main
---
---
### 1) What YOLO is solving

Traditional detection methods (like sliding windows or region proposals) are slow because they:

- Evaluate thousands of crops/regions independently.
- Repeat convolution computations unnecessarily.

**YOLO (You Only Look Once)** reframes object detection as a single regression problem:

> **"Predict bounding boxes + class probabilities directly in one forward pass."**

It is a **single-stage detector**, meaning one network pass predicts everything at once.

---

### 2) YOLO Model Output Tensor

![[Pasted image 20260116203010.png]]

YOLO divides the input image into an **$S \times S$ grid**.

Each grid cell predicts **$B$ bounding boxes** (usually defined via **[[Anchor Boxes|anchors]]**) and **$C$ class probabilities**.

The network outputs a tensor of shape:

$$\hat{y} \in \mathbb{R}^{S \times S \times B \times (5 + C)}$$

#### The Prediction Vector

For each bounding box $B$, the vector contains:

$$[P_c, b_x, b_y, b_w, b_h, c_1, c_2, \dots, c_C]$$

**Meaning of terms:**

- **$P_c$ (Objectness Score):** Probability that an object exists in this box ($Pr(\text{Object})$).
- **$(b_x, b_y)$:** Box center coordinates (relative to the grid cell bounds).
- **$(b_w, b_h)$:** Box width and height (relative to the image or anchor).
- **$c_1 \dots c_C$:** Class probabilities (conditional on object existence: $Pr(\text{Class}_i | \text{Object})$).

---

### 3) Responsibility + Assignment

Crucial to YOLO is determining _which part of the output_ is responsible for detecting a specific object.

#### 3.1 Responsibility Rule (Grid Cell)

- Find the **center** of the ground truth object.
- The specific grid cell $(i, j)$ containing this center is **responsible** for detecting the object.

#### 3.2 Anchor Assignment (Handling Overlap)

If the model uses **$B$ anchors** per grid cell:

1. Compute the **IoU (Intersection over Union)** between the Ground Truth (GT) box and all Anchor templates.
2. Assign the object to the anchor with the **highest IoU**.

**Result:** Each object is tied to a specific **Grid Cell** AND a specific **Anchor Box**. This allows a single cell to detect multiple objects if they match different anchor shapes (e.g., a tall person vs. a wide car).

---

### 4) Training Labels (Ground Truth Encoding)

For every cell and anchor, we construct a target vector $y$:

$$y = [P_c, b_x, b_y, b_w, b_h, c_1, \dots, c_C]$$

#### Target Construction Logic

1. **If the anchor is responsible for an object:**
    
    - $P_c = 1$
    - $b_x, b_y, b_w, b_h$ = GT coordinates (encoded relative to cell/anchor).
    - $c_k = 1$ (for the correct class), others $0$.
2. **If the anchor is NOT responsible (Background):**
    
    - $P_c = 0$
    - **Note:** Box parameters and class probabilities are usually ignored (masked out) in the loss function to prevent noise.

> ⚠️ **Imbalance Warning:** Most grid cells in an image do _not_ contain objects. This creates a massive class imbalance (mostly background), which loss functions must handle.

---

### 5) Loss Function (Core of YOLO Training)

YOLO optimizes a multi-part loss function combining three objectives:

$$\mathcal{L} = \lambda_{box}\mathcal{L}_{box} + \lambda_{obj}\mathcal{L}_{obj} + \lambda_{cls}\mathcal{L}_{cls}$$

#### (A) Localization Loss ($\mathcal{L}_{box}$)

Penalizes errors in the bounding box regression.

$$\mathcal{L}_{box} = \sum_{i=0}^{S^2} \sum_{j=0}^{B} \mathbb{1}^{obj}_{ij} \left[ (b_x - \hat{b}_x)^2 + (b_y - \hat{b}_y)^2 + (\sqrt{b_w} - \sqrt{\hat{b}_w})^2 + (\sqrt{b_h} - \sqrt{\hat{b}_h})^2 \right]$$

- **Note:** Square roots ($\sqrt{w}, \sqrt{h}$) are often used so that errors in small boxes matter more than errors in large boxes.
- **Modern approach:** Uses **GIoU / DIoU / CIoU** loss instead of simple MSE.

#### (B) Objectness Loss ($\mathcal{L}_{obj}$)

Does the box contain an object?

$$\mathcal{L}_{obj} = \sum_{i=0}^{S^2} \sum_{j=0}^{B} \mathbb{1}^{obj}_{ij}(1 - \hat{P}_c)^2 + \lambda_{noobj}\sum_{i=0}^{S^2} \sum_{j=0}^{B} \mathbb{1}^{noobj}_{ij}(0 - \hat{P}_c)^2$$

- Crucial for suppressing the background. $\lambda_{noobj}$ is usually small (e.g., 0.5) to prevent background from overwhelming the learning.
#### (C) Classification Loss ($\mathcal{L}_{cls}$)

Correctly identifying the class (calculated _only_ if object is present).

$$\mathcal{L}_{cls} = \sum_{i=0}^{S^2} \sum_{j=0}^{B} \mathbb{1}^{obj}_{ij} \sum_{c \in classes} (p_c(c) - \hat{p}_c(c))^2$$

- **Modern approach:** Uses **Cross Entropy** or **Focal Loss**.
---

### 6) Inference Pipeline (Test Time)

How YOLO generates final detections from a raw image:

1. **Forward Pass:** Input image $\to$ CNN $\to$ Output Tensor ($S \times S \times B \times \dots$).
2. **Decode Boxes:** Convert relative network outputs (offsets) into absolute image coordinates.
3. **Compute Scores:** Calculate the final confidence for each class: $$Score(\text{Class}_k) = P_c \times P(\text{Class}_k | \text{Object})$$    
4. **Thresholding:** Discard all boxes with $Score < \text{threshold}$ (e.g., 0.5).
5. **Non-Max Suppression (NMS):**
    - Since YOLO often predicts multiple boxes for the same object, apply [[Non-Max Suppression (NMS)|NMS]] :
        1. Pick box with highest score.
        2. Discard any other box with $IoU > 0.5$ (high overlap) with that box.
        3. Repeat for all classes.

---

### 7) Why YOLO Works Well

- **✅ Speed:** Single forward pass makes it real-time capable (45+ FPS). No complex region proposal loops.
- **✅ Global Context:** The network sees the _entire_ image at once. It produces fewer background errors (false positives) compared to sliding windows.
- **✅ Generalizable:** Learns abstract representations of objects, making it robust when applied to new domains (e.g., artwork).

---
### 8) Key Challenges / Limitations

1. **Small Objects:** Objects smaller than the grid cell can be missed, or struggle if multiple small objects appear in one cell (flocking birds).
2. **Grid Constraints:** Only $B$ objects can be detected per cell. If $>B$ objects crowd a single cell, some will be ignored.
3. **Imbalance:** The "background" dominates the image, requiring careful tuning of loss weights ($\lambda_{coord}, \lambda_{noobj}$).

---

