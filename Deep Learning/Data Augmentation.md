---
tags:
  - main
---
---

## Why Augmentation?
Computer vision models require massive amounts of data to generalize well. Augmentation increases the *effective* size of your dataset, reducing overfitting and improving generalization.

---

## Common Techniques

### 1. Horizontal Flip (Mirroring)
* **Method**: Flip image on the vertical axis.
* **Logic**: A cat facing left is still a cat; the label remains unchanged.
* **Use Case**: When orientation doesn't change the semantic meaning.

### 2. Random Cropping
* **Method**: Generate multiple crops (center, corners, random) from a single image.
* **Benefit**: Makes model robust to object position shifts and partial views.
* **Warning**: Ensure crops are large enough so they don't remove the subject entirely.

### 3. Rotation / Shear / Warping
* **Method**: Rotate, shear, or apply local distortions to the image.
* **Benefit**: Helps learn invariance to geometric changes.
* **Note**: More complex to implement and less commonly used than simple flips/crops.

### 4. Color Shifting (RGB Distortions)
* **Method**: Randomly modify R, G, B channels to mimic lighting variations (sunlight, indoor, etc.).
* **Benefit**: Robustness against illumination changes.

> [!ABSTRACT] **PCA Color Augmentation (AlexNet Trick)**
> A specific method used in AlexNet that uses PCA over RGB channels to add realistic, correlated color noise.

---

## Implementation Pipeline
To avoid bottlenecks, use a separate CPU thread for augmentation:
1.  **CPU**: Loads images and applies transforms.
2.  **GPU**: Receives ready batches for training.

## Hyperparameters
Augmentation introduces new hyperparameters to tune:
* Crop size
* Crop probability
* Max rotation angle
* RGB shift ranges

> [!TIP] Start with standard augmentation schemes from established open-source repositories rather than reinventing the wheel.