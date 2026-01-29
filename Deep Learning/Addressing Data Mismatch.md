---
tags:
  - main
---
---

When in need of more data an useful [[DL Strategy]] can to add data to the training set from a different distribution than the used for dev and test sets. Data mismatch occurs when the training data distribution differs significantly from the Dev/Test distribution.

---

## How to Handle

### 1. Manual Error Analysis
- Manually inspect the differences between the Training set and the Dev/Test set.
- Identify *what* makes them different (e.g., noise, resolution, accents).

### 2. Make Training Data Similar
- Collect more real data that matches the Dev/Test description.
- Preprocess or filter the existing training data to look closer to the target.

---

## Artificial Data Synthesis

Create synthetic training samples to match the target environment.

**Example: Audio**
- `Clean Audio` (Train) + `Car Noise` (Error Source) = **Synthesized Car-Noise Audio**

**Caution: Overfitting to Synthesis**
- If you have **10,000 hours** of clean audio but only **1 hour** of car noise effects:
- The model may overfit to that specific 1 hour of noise.
- **Solution**: Ensure the noise source is sufficiently diverse (e.g., collecting 100 hours of different car noises) even if you repeat the clean audio.