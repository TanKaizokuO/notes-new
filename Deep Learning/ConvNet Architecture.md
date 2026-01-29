---
tags:
  - main
---


Example CNN pipeline structure:



**Input:**
$$39 \times 39 \times 3$$

---

## Convolution Layer 1
**Hyperparameters:**
- $f^{[1]}=3$
- $s^{[1]}=1$
- $p^{[1]}=0$
- 10 filters

**Output:**
$$37 \times 37 \times 10$$

---

## Convolution Layer 2
**Input:**
$$37 \times 37 \times 10$$

**Hyperparameters:**
- $f^{[2]}=5$
- $s^{[2]}=2$
- $p^{[2]}=0$
- 20 filters

**Output:**
$$17 \times 17 \times 20$$

---

## Convolution Layer 3
**Input:**
$$17 \times 17 \times 20$$

**Hyperparameters:**
- $f^{[3]}=5$
- $s^{[3]}=2$
- 40 filters

**Output:**
$$7 \times 7 \times 40$$

---

**Final Step:** Flatten to vector:
$$[n_1, n_2, \dots, n_{1960}]$$