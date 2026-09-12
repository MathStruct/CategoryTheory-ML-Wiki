#exercise #solution

**Exercise 1.114.** In Example 1.113 ($P = \{1, 2 \leq 3.9 \leq 4\}$ with $1, 2 \leq 3.9$; $Q = \{1, 2 \leq 4\}$; $g$ the inclusion, $f$ rounding $3.9$ to $4$), check the twelve conditions $f(p) \leq q$ iff $p \leq g(q)$.

## Solution

| $p$ | $q$ | $f(p) \leq q$ | $p \leq g(q)$ |
|---|---|---|---|
| 1 | 1 | yes | yes |
| 1 | 2 | no | no |
| 1 | 4 | yes | yes |
| 2 | 1 | no | no |
| 2 | 2 | yes | yes |
| 2 | 4 | yes | yes |
| 3.9 | 1 | no | no |
| 3.9 | 2 | no | no |
| 3.9 | 4 | yes | yes |
| 4 | 1 | no | no |
| 4 | 2 | no | no |
| 4 | 4 | yes | yes |

All agree, so $f \dashv g$; yet $g$ does not preserve joins ([[Right Adjoints Preserve Meets]] but not joins).
