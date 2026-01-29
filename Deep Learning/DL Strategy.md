---
tags:
  - umbrella
---

---
### 1. [[Orthogonalisation]]

The core philosophy is to ensure that the controls you have for your system work independently. This allows you to debug and improve specific parts of the pipeline without breaking others.

**The Pipeline:**
1.  Fit the **training set** (Bias).
2.   Fit the **dev set** (Variance).
3.   Fit the **test set** (Generalization)..        
4. Perform well in the **real world**.

### 2. [[Balancing Metrics]]

To make progress, you need a clear way to rank models.

- **Single Number Metric:** It is difficult to choose between models if you are comparing multiple numbers (e.g., Precision and Recall separately). Using a combined metric like **F1 Score** (harmonic mean) allows for instant ranking.

- **Optimizing vs. Satisficing:**
    
    - **Optimizing Metric:** The metric you want to maximize (e.g., Accuracy). You generally only have one of these.
    
    - **Satisficing Metric:** A constraint that must be met (e.g., Runtime $\le$ 100ms). As long as it is met, "better" doesn't matter.
    

### 3. [[Dataset Split]]

- **Distribution:** The most critical rule is that your **Dev and Test sets must come from the same distribution**. They should represent the data you expect to encounter in the future.

- **Sizing:** The Test set must be large enough to give high confidence in the evaluation.

- **Flexibility:** If your metric (e.g., error rate) favors a model that fails in the real world (e.g., allowing explicit content), you must change your metric or your Dev/Test sets.


### 4. [[Setting and Adjusting Targets]]

Comparing your model to human performance helps diagnose **Bias vs. Variance** issues.

- **Bayes Error:** The theoretical limit of model performance. Human-level error is often used as a proxy for this.

- **Avoidable Bias:** The gap between **Human Error** and **Training Error**. If this is large, focus on training bigger networks or training longer.

- **Variance:** The gap between **Training Error** and **Dev Error**. If this is large, focus on regularization or getting more data.


Example Scenario:

If Human Error is 1%, Train Error is 8%, and Dev Error is 10%:

- The **Avoidable Bias** is 7% ($8\% - 1\%$).

- The **Variance** is 2% ($10\% - 8\%$).

- **Strategy:** Focus on reducing Bias (the larger gap) rather than Variance. 

---

[[]]