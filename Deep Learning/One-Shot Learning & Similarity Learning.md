---
tags:
  - extension
---

## One-Shot Learning Problem
The goal is to recognize a person given only **one** example image of them.



**Why Softmax Classification Struggles**:
* It requires a fixed set of identities.
* Adding a new person requires retraining the classifier.
* It does not scale well in real-world scenarios.

## Solution: Learn a Similarity Function
Instead of learning to classify identities directly, we learn a function $d$:
$$d(\text{img}_1, \text{img}_2) = \text{degree of difference between two images}$$

* If $d$ is **small** → Same person.
* If $d$ is **large** → Different persons.

This is implemented using a [[Siamese Network]].