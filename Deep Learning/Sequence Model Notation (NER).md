---
tags:
  - main
---
---

**NER** = assign labels to each word in a sentence.

**Example**:
$x =$ "Harry Potter and Hermione Granger invented a new spell."
$y =$ 1 1 0 1 1 0 0 0 0

---

## Notation
![[Pasted image 20260118233752.png]]
* $x$: input sentence (sequence).
* $y$: output sequence (labels).

**At timestep $t$**:
* $x^{\langle t\rangle}$: t-th word.
* $y^{\langle t\rangle}$: label for t-th word.

**For i-th training example**:
* $x^{(i)\langle t\rangle}$: word at timestep $t$.
* $T_x^{(i)}$: length of input.

**Output length**:
* $T_y$: output sequence length.
* For NER: $T_x = T_y$.

![[Pasted image 20260118233942.png]]

---

## Word Representation

### One-Hot
**Steps**:
1.  Build vocabulary/dictionary.
2.  Convert each word → one-hot vector.

**Unknown word**:
* Special token `<UNK>`.

This representation is used in:
* [[Vanilla RNN Model]]
* [[Language Model & Sequence Generation]]