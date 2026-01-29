---
tags:
  - main
---
---

Neural Style Transfer generates an image $G$ that preserves the **content** from a content image $C$ while transferring the **style** from a style image $S$.


**Core Idea**: Use a pretrained CNN (often VGG) and compare intermediate activations to extract features.

**Paper**: "A Neural Algorithm of Artistic Style".

---

## What CNN Layers Learn
* **Shallow layers** → Edges, textures.
* **Deeper layers** → Object structure / semantics.

---

## Objective Function
$$J(G) = \alpha \cdot J_{\text{content}}(C, G) + \beta \cdot J_{\text{style}}(S, G)$$

**Procedure**:
1.  Initialize generated image $G$ randomly (e.g., $100 \times 100 \times 3$).
2.  Run gradient descent to minimize $J(G)$.
3.  Output optimized $G$.

---

## Content Cost
Choose a layer $l$ (usually a mid-level layer).
* $a^{[l](C)}$ = Activation on content image.
* $a^{[l](G)}$ = Activation on generated image.

$$J_{\text{content}}(C,G) = \frac{1}{2} || a^{[l](C)} - a^{[l](G)} ||^2$$

---

## Style Cost
Style is defined as the correlation across channels.

**Method**:
1.  Compute the **Gram matrix** of activations.
2.  Match Gram matrices between $S$ and $G$.

**Effect**: Transfers texture, patterns, brush strokes, and color distributions.