---
tags:
  - main
---
---

## Region Proposals or R-CNN (Regions with CNN features)
Instead of processing the whole image or a grid, find "interesting" blobs first.
1.  **Propose Regions**: Use a segmentation algorithm (like Selective Search) to find ~2000 blobs.
2.  **Classify**: Run a standard ConvNet on each region independently.
❌ **Cons**: Very slow (ConvNet runs 2000 times).

---

## Fast R-CNN
1.  **Propose Regions**: Use Selective Search.
2.  **ConvNet**: Run the image through ConvNet **once** to get a feature map.
3.  **ROI Pooling**: Project the proposed regions onto the feature map and classify.
❌ **Cons**: The "Region Proposal" step (Selective Search) is still slow and not learned.

---

## Faster R-CNN
### Key Innovation: Region Proposal Network (RPN)
Instead of Selective Search, use a specialized **neural network** to propose regions.
1.  Run Image $\to$ ConvNet $\to$ Feature Map.
2.  **RPN** predicts region proposals from the feature map.
3.  Classify those regions.

✅ **Pros**: Much faster than R-CNN.
❌ **Cons**: Still typically slower than [[YOLO Algorithm|YOLO]] for real-time applications.

---
