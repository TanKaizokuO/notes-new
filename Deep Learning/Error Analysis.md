---
tags:
  - main
---
---

Error analysis is an [[DL Strategy]] which helps identify what types of mistakes the model is making and what improvements will give the biggest gain.

---

## Example Scenario

**Model**:
- 90% accuracy (10% error)
- Task: Cat classifier

**Observation**:
Some dogs are being misclassified as cats.

**Question**: **What should you do?**

---

## Correct Approach (Systematic)

❌ **Do NOT** immediately start building special features for dogs (guessing).

✅ **First perform systematic error analysis**:
1. Collect **100 random misclassified examples** (Dev set).
2. Manually inspect them.
3. Categorize the error types.

---

## Case Outcomes

### Case A: Dog problem is rare
- **Result**: Only 5 out of 100 errors are dogs (5%).
- **Conclusion**: Even if you completely solve the dog problem, error only drops from 10% to 9.5%. **Do NOT focus on dogs.**

### Case B: Dog problem is common
- **Result**: 50 out of 100 errors are dogs (50%).
- **Conclusion**: Solving this could reduce error from 10% to 5%. **Focus on solving dog confusion.**

---

## Create an Error Analysis Spreadsheet

**Example Sheet:**

| Image ID | Dog | Great Cat | Blurry | Comments |
| :--- | :---: | :---: | :---: | :--- |
| 1 | ✅ | | | Looks like a pitbull |
| 2 | | ✅ | ✅ | Lion in rain |
| ... | ... | ... | ... | ... |
| 100 | | ✅ | | Leopard |

> **Tip**: Add more columns (categories) as you discover them during inspection.

---

## Cleaning Incorrectly Labelled Data

### Training Set
Deep learning algorithms are fairly robust to **random errors** in training labels.
- **Systematic mislabeling** (e.g., all white dogs labelled as cats) is problematic and needs fixing.

### Dev / Test Set
If dev/test labels are wrong, evaluation becomes unreliable.

✅ **Action**: Add a column in your error analysis for "Incorrectly Labelled".

---

## Label Correction Notes

If you decide to fix mislabeled data:
1. Apply the same correction process to **both Dev and Test** sets to maintain distribution consistency.
2. Consider examining examples the model got *right* as well (to ensure you aren't only fixing "hard" cases).
3. **Note**: After correction, Train and Dev/Test sets may come from slightly different distributions (if you only fix Dev/Test). This is usually acceptable.