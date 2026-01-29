---
tags:
  - main
---
---
# Train / Dev / Test Set Strategy
It is very important to split the dataset appropriately.  

## Key Guidelines

### 1. Dev and Test must come from same distribution
They should reflect the data expected in the future (the target distribution).

### 2. Test set must be large enough
So performance evaluation has **high confidence**.

---

## When to change dev/test sets and metrics

If dev/test performance does **NOT** reflect real-world performance preferences, then we must change:
- The [[Evaluation Metrics]]
- The dev/test sets themselves

---

## Example: Cat Classifier Case

* **Algo A**: 3% error
* **Algo B**: 5% error

**Issue**: **A allows pornographic images to pass** (bad real-world behavior), while B does not.

✅ **Choose B**, because the dev metric (and cost function) must reflect application needs (blocking unsafe content).