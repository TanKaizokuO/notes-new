---
tags:
  - extension
---
---
![[Selection_002.png]]
Depthwise separable convolution reduces compute cost by splitting standard convolution into two distinct stages.

---

## Standard Convolution Cost
Input: $H \times W \times M$
Filters: $N$ filters of size $K \times K$

**Cost:**
$$H \cdot W \cdot K^2 \cdot M \cdot N$$


---

## Depthwise Separable Convolution

### Step 1: Depthwise Convolution
Apply one $K \times K$ filter per input channel.

**Cost:**
$$H \cdot W \cdot K^2 \cdot M$$


### Step 2: Pointwise Convolution ($1\times1$)
Mix the channels to produce $N$ output channels.

**Cost:**
$$H \cdot W \cdot M \cdot N$$


---

## Total Cost
$$H\cdot W(K^2M + MN)$$


---

## Efficiency Ratio
$$\frac{K^2M + MN}{K^2MN} = \frac{1}{N} + \frac{1}{K^2}$$

**Example:**
- Standard cost = 2160
- Depthwise cost = 672
- Ratio: $\frac{672}{2160} \approx 0.31$

✅ **~31% of the computation** (approx. 69% reduction).