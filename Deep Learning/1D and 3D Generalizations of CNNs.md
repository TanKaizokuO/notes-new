---
tags:
  - main
---
---

CNNs generalize beyond 2D images to other dimensions.
## 1D Convolution
Used for time-series signals like ECG data.

* **Example**:
    * Input: $14 \times 1$
    * Filter: $5 \times 1$
    * Output: $10 \times 16$ (16 filters)

**Usage**: Often combined with sequential models like RNNs, LSTMs, or Transformers.

## 3D Convolution
Used for volumetric data such as:
* CT scans
* MRI
* Medical segmentation
* Video (where time acts as the depth dimension)

* **Example**:
    * Input: $14 \times 14 \times 14 \times 1$
    * Filter: $5 \times 5 \times 5 \times 1$
    * Output: $10 \times 10 \times 10 \times 16$