---
tags:
  - main
---
---

EfficientNet proposes a principled method to scale Convolutional Neural Networks (CNNs) to maximize accuracy within specific computational constraints.

---

## The Problem
Different devices (mobile phones, IoT, servers) have vastly different compute and memory limits.
- Manually designing a specific architecture for every resource budget is inefficient.
- Arbitrarily scaling just one factor (e.g., just adding more layers) often yields diminishing returns.

---

## The Solution: Compound Scaling
Starting from a baseline model, EfficientNet scales the network uniformly along **three dimensions** simultaneously:



1.  **Resolution ($r$)**: Changing the input image size (higher resolution = finer patterns).
2.  **Depth ($d$)**: Increasing/decreasing the number of layers (deeper = more complex features).
3.  **Width ($w$)**: Increasing/decreasing the number of channels per layer (wider = more fine-grained features).

---

## The Optimization
The core question EfficientNet answers is:
> **Given a specific compute budget (FLOPS/Memory), what is the optimal trade-off between $r, d$, and $w$?**

Instead of tuning these independently, EfficientNet finds a compound coefficient to scale all three together in a balanced way.

---

## Comparison: MobileNet vs. EfficientNet

| Model | Primary Focus |
| :--- | :--- |
| **MobileNet** | **Efficient Layer Design**: Provides the fundamental building blocks (e.g., Depthwise Separable Convs) to make individual layers cheap. |
| **EfficientNet** | **Efficient Scaling Strategy**: Provides the best way to expand or shrink the model size to fit a specific device budget. |