---
tags:
  - extension
---
---
# Local Binary Patterns (LBP)

LBP is simple and powerful for:
- texture classification
- face recognition

## Procedure
For each pixel:
1. take 3×3 neighborhood
2. compare neighbor vs center:
   - neighbor ≥ center → 1
   - else → 0
3. read 8-bit binary pattern
4. convert binary → decimal = LBP value

Encodes local texture:
- flat
- edge
- corner

**Backlinks:** [[Image Features]]

**Source:** :contentReference[oaicite:18]{index=18}
