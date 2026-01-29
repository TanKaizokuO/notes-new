---
tags:
  - main
---
---
## Motivation
Naive sliding windows waste computation because many windows overlap, causing the ConvNet to re-calculate features for the same pixels multiple times.

---

## Key Trick
Convert **Fully Connected (FC)** layers into **Convolutional (CONV)** layers.
![[Pasted image 20260116200050.png]]


- **Naive**: Run model separately for each crop.
- **Convolutional**: Run the model **once** over the entire image. The final output volume effectively contains the classification results for all "sliding windows" simultaneously.
![[Pasted image 20260116200112.png]]

---

## Benefits
✅ **Efficiency**: Computation is shared; feature maps are calculated only once per image.
✅ **Speed**: Much faster than naive sliding windows.
✅ **Simplicity**: The whole image is processed in a single forward pass.