---
tags:
  - main
---
---
# Human-Level Performance and Avoidable Bias

![[IMG_0981.png]]
## Comparing to Human Performance

As long as model performance is worse than humans, you can:
- Collect labelled data from humans.
- Analyze manual errors to gain insights.
- Improve bias/variance diagnosis.

---

## Avoidable Bias

Avoidable bias is approximately the difference between:
1.  **Human-level error** (proxy for Bayes error)
2.  **Training error**

$$Avoidable\ Bias = Training\ Error - Human\ Error$$

> **Note**: Do not try to beat Bayes error; beyond that point, you are likely just overfitting.

---

## Human Error as Proxy for Bayes Error

**Example**: Medical image classification
- Typical human: 3% error
- Typical doctor: 1% error
- Experienced doctor: 0.7% error
- Team of experienced doctors: 0.5% error

**Human-level error** can be defined as:
- **0.5%** (best achievable human performance)
- or **1%** (depending on context, e.g., for a specific application vs. research paper)

Thus:
$$Bayes\ Error \le 0.5\%$$

---

## Example Bias/Variance Decomposition

**Given:**
- Training error = 5%
- Dev error = 6%
- Human error = 0.5%

**Analysis:**
1.  **Avoidable Bias**: $5\% - 0.5\% = 4.5\%$
2.  **Variance**: $6\% - 5\% = 1\%$

✅ **Conclusion**: Focus should be on **bias reduction** (bigger model, train longer, better architecture), as avoidable bias (4.5%) > variance (1%).

---

## Avoidable Bias Scenarios

| Scenario | Human Error | Train Error | Dev Error | Diagnosis                                                                         |
| :------- | :---------: | :---------: | :-------: | :-------------------------------------------------------------------------------- |
| **A**    |     1%      |     8%      |    10%    | Large avoidable bias ($8\%-1\%=7\%$). Focus on **Bias**.                          |
| **B**    |    7.5%     |     8%      |    10%    | Small avoidable bias ($8\%-7.5\%=0.5\%$). Focus on **Variance** ($10\%-8\%=2\%$). |