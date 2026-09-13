#solution

**Solution to [[7S Exercise 4.22|Exercise 4.22]].**

All shortest paths go through the bridges $D \to y$ (length 9) and $y \to r$ (length 0), so $(\Phi \mathbin{;} \Psi)(-, -) = X(-, D) + 9 + Z(r, -)$:

| | p | q | r | s |
|---|---|---|---|---|
| A | 22 | 24 | 20 | 21 |
| B | 16 | 18 | 14 | 15 |
| C | 19 | 21 | 17 | 18 |
| D | 11 | 13 | 9 | 10 |

Alternatively $\Phi \ast M_\Psi \ast M_Z^3$ by min-plus multiplication. See [[Category of Profunctors]].

> Sources: 7 Sketches, Exercise 4.22 and Solution A.4.
