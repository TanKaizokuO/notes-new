---
tags:
  - extension
---

## Verification (1:1 Matching)
* **Input**: An image + a claimed name/ID.
* **Output**: Whether the image is that claimed person.
* **Example**: "Is this person Tanishq?" → Yes/No.

## Recognition (1:K Matching)
* **Database**: $K$ persons.
* **Input**: An image.
* **Output**: The predicted ID if a match is found, otherwise "not recognized".

> [!INFO] Difficulty
> Recognition is harder than verification because it requires comparing the input against many identities rather than just one.

**Next**: [[One-Shot Learning & Similarity Learning]]