---
tags:
  - main
---
---
# AlexNet (2012)

**Purpose:** Won ImageNet 2012 and triggered the modern deep learning boom. It demonstrated that deep CNNs could be trained effectively on GPUs to solve complex tasks.

---

## 1. Input Layer
- **Dimensions:** $227 \times 227 \times 3$
- **Explanation:** Accepts RGB images. Raw images are often cropped or resized to these dimensions.

---

## 2. Convolutional Layers (Feature Extraction)
 The first five layers extract features, moving from low-level edges to high-level shapes.

### Layer 1 (Conv1)
- **Operation:** $11 \times 11$ filters, stride $s=4$.
- **Output:** $55 \times 55 \times 96$
- **Insight:** Large filters capture broad, low-frequency features. The aggressive stride of 4 downsamples spatial dimensions early to reduce computation.
- **Activation:** Followed by **ReLU** and **Overlapping Max Pooling** ($3 \times 3, s=2$).

### Layer 2 (Conv2)
- **Operation:** $5 \times 5$ filters, padding="same".
- **Output:** $27 \times 27 \times 256$
- **Insight:** Depth increases (96 $\to$ 256), allowing the network to learn a wider variety of textures.
- **Activation:** Followed by **ReLU** and **Max Pooling**.

### Layers 3, 4, and 5 (Deep Features)
- **Conv3:** $3 \times 3$ filters $\to$ Output $13 \times 13 \times 384$.
- **Conv4:** $3 \times 3$ filters $\to$ Output $13 \times 13 \times 384$.
- **Conv5:** $3 \times 3$ filters $\to$ Output $13 \times 13 \times 256$.
- **Insight:** Stacked directly without pooling. This allows the network to learn complex, high-level semantic representations (e.g., eyes, wheels) before final downsampling.

---

## 3. Fully Connected Layers (Classification)
The final pooling layer outputs $6 \times 6 \times 256$, which is flattened into a vector of **9,216** values.

### FC6 & FC7 (Dense Layers)
- **Size:** 4096 neurons each.
- **Key Innovation (Dropout):** Randomly drops 50% of neurons during training. This prevents overfitting by forcing the network to learn robust features that don't rely on specific single neurons.

### FC8 (Output Layer)
- **Size:** 1000 neurons (for ImageNet).
- **Operation:** Softmax function.
- **Output:** Probability distribution across 1,000 classes (e.g., "cat", "ship").

---

## Summary of Metrics
- **Total Parameters:** $\approx 60$ Million.
    - *Note:* Most parameters reside in the dense connection between the flattened Conv5 output and FC6.
- **Depth:** 8 learnable layers (5 Conv + 3 FC).

---

## Why this architecture worked?
1.  **ReLU vs. Sigmoid:** Replaced Tanh/Sigmoid with ReLU ($f(x) = \max(0, x)$). ReLU does not suffer from vanishing gradients, speeding up convergence by **6x**.
2.  **GPU Utilization:** Designed to be split across two GPUs (originally GTX 580s) because a single GPU lacked sufficient VRAM.
3.  **Data Augmentation:** Used cropping and flipping to artificially expand the dataset and reduce overfitting.