---
tags:
  - main
---
---

Image features are distinctive patterns/structures in an image used for:
- matching
- recognition
- tracking

## Types of Image Features
### 1) Corners
High variation in both directions
- Example: Harris corner

### 2) Edges
Strong intensity change in one direction
- Example: Sobel, Canny

### 3) Blobs / Regions
Connected pixels with similar properties
- Example: DoG, LoG

### 4) Texture Patterns
Repetitive intensity/color patterns
- Example: LBP, Gabor filters

### 5) Keypoints in Scale-Space
Stable points across scales
- include: location, scale, orientation, descriptor vector
- Example: SIFT, SURF, ORB

### 6) Color Features
- histograms (RGB/HSV)
- color moments

**Backlinks:** [[SIFT]] [[Harris Corner Detector]] [[LBP]] [[HoG]]


---
# Canny Edge Detector
Canny (1986) is one of the most widely used edge detectors in computer vision.

**Backlinks:** [[Edge Detection Filters]] [[Image Features]]

---