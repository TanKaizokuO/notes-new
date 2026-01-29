---
tags:
  - main
---
---
## Definition
![[Pasted image 20260116195803.png]]

Instead of outputting a simple box, the neural network outputs specific $(x, y)$ coordinates for key points on the object. These points are called **landmarks**.

---

## Examples

### 1. Face Landmarks
Used for filters (Snapchat/Instagram) or emotion recognition.
- Left/Right eye corners
- Nose tip
- Mouth edges
- Jawline contour

### 2. Pose Landmarks
Used for activity recognition or motion capture.

- Chest midpoint
- Shoulders, Elbows, Wrists
- Hips, Knees, Ankles

---

## Key Rule: Consistency
**Identity consistency is crucial**:
- Landmark #1 must *always* represent the same point (e.g., left eye corner) in every image.
- Landmark #2 must *always* represent the next point (e.g., right eye corner), and so on.