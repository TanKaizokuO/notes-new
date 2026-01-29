---
tags:
  - extension
---
---
# SIFT (Scale Invariant Feature Transform)

SIFT identifies keypoints and creates descriptors invariant to:
- scale
- rotation
- translation
- (partially) perspective
- illumination changes (robust)

Each keypoint stores:
- location (x,y)
- scale σ
- orientation θ
- descriptor vector (128D)

---

## Key Steps
1. Scale-space extrema detection (DoG)
2. Keypoint localization (refine + remove unstable points)
3. Orientation assignment
4. Descriptor construction (128D)

---

## Applications
### Image Stitching
- detect SIFT keypoints in both images
- match descriptors
- align images and stitch panorama

**Backlinks:** [[Image Features]] 

**Source:** :contentReference[oaicite:16]{index=16}
