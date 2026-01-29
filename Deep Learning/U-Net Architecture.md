---
tags:
  - main
---
---

## U-Net Overview

![[Selection_004.png]]

U-Net is an encoder–decoder CNN with skip connections. 

Shape:
* left side: encoder (downsampling)
* bottom: bottleneck (lowest resolution, deepest features)
* right side: decoder (upsampling)

---

## Input
* Input image: **H × W × 3** (RGB).
* Goal: output a segmentation map of size **H × W × C**.

---

## Encoder (Contracting Path)
The encoder consists of repeated blocks:

### Conv Block
* Conv (3×3) + ReLU
* Conv (3×3) + ReLU

### Downsampling
* MaxPool (2×2)

After each downsampling, height and width reduce and channels increase (e.g., H×W×64 → H/2 × W/2 ×128).

---

## Bottleneck
Lowest spatial resolution and largest channel depth. This represents high-level semantic meaning (object presence + rough location).

---

## Decoder (Expanding Path)
Repeated blocks:

### Upsampling
* [[Transpose Convolutions]] (transpose convolution).
* Effect: increases H and W.

### Skip connection concat
* concatenate features from matching encoder layer.

### Conv Block
* Conv + ReLU
* Conv + ReLU

---

## Skip Connections in detail
At each decoder stage, take encoder activation with same spatial size and concatenate with upsampled decoder activation.

**Why?**
* ✅ preserves fine edges and textures
* ✅ improves localization accuracy
* ✅ produces sharper segmentation

---

## Final Layer
### 1×1 Convolution
Maps channels into number of classes `C`.

Output: **H × W × C**.

Then apply argmax along channel dimension → class mask.

---

## Summary
U-Net achieves strong segmentation because encoder captures context, decoder reconstructs pixel map, and skip connections restore lost spatial detail.