---
tags:
  - main
---
---
## What is Semantic Segmentation?

![[Selection_001.png]]

Semantic segmentation is a computer vision task where the model assigns a **class label to every pixel** of an input image.

This is more detailed than:
* **Object Recognition (Classification):** predicts one label for the whole image.
* **Object Detection:** predicts label + bounding box.
* **Semantic Segmentation:** predicts **pixel-wise labels**.

✅ Output is a **segmentation mask / segmentation map** of size **H × W** (or **H × W × C** for multi-class).

---

## Why is it useful?
Semantic segmentation helps solve problems where bounding boxes are not enough, such as identifying **exact object boundaries**.

### Example: Self-Driving Cars
* **Object detection:** draws bounding boxes around vehicles.
* **Semantic segmentation:** labels each pixel as road (drivable), car, sidewalk, or background.

✅ Helps determine exactly which pixels represent **drivable surface** for safer navigation decisions.

---

## Medical Imaging Use Cases
Semantic segmentation is widely used in healthcare.

![[Selection_002 2.png]]
### 1) Chest X-ray segmentation
Labels anatomical regions like lungs, heart, and clavicle (collarbones).
* **Benefits:** highlights irregularities, supports diagnosis, and helps in surgery planning.

### 2) Brain MRI tumor segmentation
Automatically identifies tumor pixels.
* **Benefits:** manual segmentation is slow; automation improves workflow for radiologists and assists surgical planning.

---

## Semantic Segmentation Labels
### Binary segmentation
* Classes: 1 = car, 0 = background.
* Each pixel predicts {0,1}.

### Multi-class segmentation
* Example: 1 = car, 2 = building, 3 = road.
* Each pixel predicts one of the classes.

---

## Why U-Net is used
Semantic segmentation needs:
* **high-level understanding** (what object is present).
* **high resolution output** (exact pixel boundaries).

U-Net solves this by compressing the image (encoder), expanding it back (decoder), and using **skip connections** to preserve detail.

---

## Key Idea of U-Net
U-Net outputs a volume of **H × W × C**, where H, W = input height and width, and C = number of classes.

Prediction per pixel:
* class probabilities.
* final label = `argmax(probabilities)`.