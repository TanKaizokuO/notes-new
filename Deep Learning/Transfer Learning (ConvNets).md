---
tags:
  - main
---
---

## What is Transfer Learning?
**[[Transfer Learning]]** is the process of reusing a neural network pre-trained on a massive dataset (e.g., ImageNet, COCO, Pascal) and adapting it to a specific task.



> [!INFO] Why use it?
> * **Time Efficiency**: Pretraining from scratch can take weeks and requires multi-GPU setups.
> * **Better Initialization**: You start with strong, learned feature detectors (edges, textures, shapes) instead of random noise.

---

## Example Scenario: "Tigger vs Misty vs Neither"
**Task**: Classify an image into 3 classes (Tigger, Misty, Neither).
**Problem**: You have a small dataset and not enough images of these specific cats.

**The Solution**:
1.  Download a pretrained model + weights (e.g., trained on ImageNet).
2.  Remove the final **softmax layer** (which usually outputs 1000 classes).
3.  Replace it with your own **softmax layer** (outputting 3 classes).

---

## Three Common Transfer Learning Settings

### 1. Small Dataset Strategy (Freeze All)
> [!TIP] Best when you have very few labeled images.
> * **Action**: Freeze **all** convolution blocks.
> * **Train**: Only the new softmax head.
> * **Framework**: `trainable = False` or `freeze = 1`.

### 2. Medium Dataset Strategy (Fine-tune Some)
> [!TIP] Best when dataset is moderate or slightly different from ImageNet.
> * **Action**: Freeze the early layers (generic features).
> * **Train**: The last few convolution blocks + the output head.

### 3. Large Dataset Strategy (Train All)
> [!TIP] Best when you have lots of data or the task is significantly different.
> * **Action**: Use pretrained weights as initialization.
> * **Train**: Fine-tune **all** layers.

---

## Speed Trick: Precompute Activations
If you are freezing most layers:
1.  Compute feature vectors for your dataset **once** by running them through the frozen layers.
2.  Store these vectors to disk.
3.  Train a shallow classifier head directly on these saved features.

> [!SUCCESS] **Benefit**: drastically faster training as you avoid repeated forward passes through the deep frozen network.

---

## Key Takeaway
> [!NOTE] Rule of Thumb
> Transfer learning is **almost always worth using** in computer vision unless you have a huge dataset AND a huge compute budget.

**Next**: See [[Data Augmentation]] to further improve performance on small datasets.