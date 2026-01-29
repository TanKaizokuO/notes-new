---
tags:
  - main
---
---

## The Research Gap
The Computer Vision research community is driven by **benchmarks** (leaderboards, competitions). This incentive structure leads to the development of "tricks" that boost scores but are impractical for real-world production.

---

## Common Benchmark Tricks (Avoid in Prod)

### 1. Ensembling
* **Method**: Train multiple independent networks (e.g., 3, 5, or 7) and average their outputs.
* **Pros**: Improves accuracy by ~1–2%.
* **Cons**: Slows inference by 3–15x; increases compute and memory usage massively.

### 2. Multi-crop at Test Time
* **Method**: Run the test image through the network multiple times using different crops (e.g., 10-crop: center + 4 corners, plus their mirrors) and average the predictions.
* **Pros**: Boosts benchmark scores.
* **Cons**: Significantly slower inference.



---

## Production Advice

For real-world deployment, prefer efficiency over the last 1% of accuracy:

> [!CHECK] **Do This**
> * Use a **single**, strong pretrained model.
> * Fine-tune appropriately.
> * Use reasonable [[Data Augmentation]].

> [!FAIL] **Avoid This**
> * Heavy inference tricks (ensembling, multi-crop) unless accuracy is mission-critical and you have a massive compute budget.