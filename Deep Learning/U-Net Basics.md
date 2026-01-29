---
tags:
  - main
---
---

## What is U-Net?
U-Net is a [[CNN Archeitectures|convolutional neural network architecture ]]designed for **semantic segmentation**. It was originally introduced for [[Semantic Segmentation|biomedical image segmentation]], but is now used broadly. It is called **U-Net** because its structure looks like the letter **U**.

---

## Why U-Net works well for segmentation
Segmentation requires detailed spatial output (pixel-wise) and understanding object context.
![[Selection_003 1.png]]
But deep CNNs lose spatial detail due to pooling and shrink H and W in deeper layers.

U-Net fixes this by:
* ✅ using encoder-decoder design
* ✅ using skip connections for fine detail

---

## Main Components of U-Net

### 1) Encoder (Contracting Path)
* **Purpose:** compress input and learn high-level features.
* **Operations:** convolution + ReLU, max pooling.
* **Effect:** spatial size decreases, channels increase.

### 2) Decoder (Expanding Path)
* **Purpose:** reconstruct high-resolution output.
* **Operations:** [[Transpose Convolutions]] (upsampling), concat skip features, convolution + ReLU.
* **Effect:** spatial size increases, channels decrease.

---

## Skip Connections
Skip connections copy encoder features to decoder.

**Why needed?**
* deep layers contain context (what).
* early layers contain detail (where exactly).

Skip connections allow decoder to use both high-level context and fine-grained boundary detail.

---

## Output
* Final output size: **H × W × C**.
* Each pixel contains probabilities for each class.
* Final pixel label: `argmax()` over the class probabilities.