---
tags:
  - extension
---
---
# LoG as Blob Detector (Laplacian of Gaussian)

## Idea
Convolution is like comparing a small template against all local regions.

## Why LoG works for blobs
- LoG filter is circularly symmetric → ideal for blob-like structures
- blob = connected region of pixels with similar brightness/color/texture that stands out

Applications:
- counting structures in images (sunflowers example)
- medical imaging (RBC detection, lesions)

**Backlinks:**  [[Image Features]]