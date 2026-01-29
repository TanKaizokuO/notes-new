---
tags:
  - main
---
---
## Idea
Run a standard classifier (ConvNet) on multiple cropped regions (windows) of the image to find the object.

![[Pasted image 20260116195836.png]]

### Algorithm
1.  Select a **window size**.
2.  Slide the window across the image with a specific **stride**.
3.  Crop that region and feed it to the ConvNet.
4.  Classify: Object or Background?

---

## Advantages
- **Simplicity**: Very easy to understand and implement conceptually.

---

## Disadvantages
- **Computationally Expensive**: You are running the full ConvNet hundreds or thousands of times for a single image.
- **Localization Accuracy**:
    - Large stride $\to$ Poor localization (might miss the object center).
    - Small stride $\to$ Explodes computation cost.