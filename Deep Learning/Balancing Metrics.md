---
tags:
  - main
---
---
# Satisficing and Optimizing Metrics

In practice, we often care about multiple objectives simultaneously.

---

## Optimizing Metric
The metric we want to **maximize** (or minimize).
* **Example**: Accuracy (maximize)

---

## Satisficing Metric
The metric that must meet a specific **threshold**; once met, further improvement isn't prioritized over the optimizing metric.
* **Example**: Runtime (must be $\le 100\text{ms}$)

---

## Example Table

| Model | Accuracy (Optimizing) | Runtime (Satisficing) |
| :--- | :--- | :--- |
| A | 90% | 85 ms |
| B | 92% | 95 ms |
| C | 95% | 1500 ms |

**Constraint**: Runtime $\le 100\text{ms}$.

✅ **Best model is B** (highest accuracy while meeting the runtime constraint).