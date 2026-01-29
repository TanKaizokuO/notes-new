---
tags:
  - main
---
---

# Categorical Encoding

Categorical data must be converted into numerical form before applying most ML algorithms.

---

## Types of Data
### 1) Numerical
- **Integers / Floats**
- *Examples*: Age, Salary, Temperature

### 2) Categorical
Split into:
- **Nominal** (No order)
- **Ordinal** (Ordered)

---

## Nominal Data
Nominal categories have **no intrinsic order**.
- *Examples*: Gender (Male, Female), Colors (Red, Blue).
- **Technique**: **One-Hot Encoding (OHE)**

### Example: One-Hot Encoding
**Feature**: Color = {Yellow, Blue, Red}


- Yellow → `[1, 0, 0]`
- Blue → `[0, 1, 0]`
- Red → `[0, 0, 1]`

---

## Ordinal Data
Ordinal categories have a **meaningful order** or rank.
- *Examples*: Size (S < M < L), Education (HS < UG < PG).
- **Technique**: **Ordinal Encoding**

### Example: Ordinal Encoding
**Order**: $PG > UG > HS$
- HS → 0
- UG → 1
- PG → 2

---
