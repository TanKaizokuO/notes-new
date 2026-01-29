---
tags:
  - main
---
---

MobileNet architectures are optimized for **mobile/edge devices** where compute and memory are limited.

---

## MobileNetV1
![[Architecture-of-MobileNet-model.png]]
### Core Idea
✅ Use [[Depthwise Separable Convolution]] to drastically reduce parameters and compute.

### Benefits
- Low computation
- Low memory usage
- Good accuracy for mobile deployment

---

## MobileNetV2 Improvements

![[MobileNet-V2.png]]

### ✅ 1) Inverted Residuals
- **ResNet Approach**: Wide $\to$ Narrow $\to$ Wide (bottleneck in middle)
- **MobileNetV2 Approach**: Narrow $\to$ Wide $\to$ Narrow (expansion in middle)

This structure saves compute while retaining performance.

### ✅ 2) Linear Bottleneck
The final projection layer uses **no ReLU** (linear activation), because ReLU can destroy information in low-dimensional spaces (the "narrow" parts of the network).

---

## MobileNetV2 Block Structure
1.  **$1\times1$ Conv**: Expand channels (Wide)
2.  **Depthwise Conv**: Filtering
3.  **$1\times1$ Conv**: Project back to low dimensions (Linear / Narrow)