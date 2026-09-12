#exercise #solution #proof

**Exercise 7.29.** 1. Verify that the coarse topology $\{\varnothing, X\}$ is a [[Topological Space|topology]]. 2. Verify that the fine topology $\mathcal{P}(X)$ is. 3. Show every function from a discrete space $(X, \mathcal{P}(X))$ to any space $Y$ is continuous.

## Solution

1. It contains $X$ and $\varnothing$; $A \cap B = \varnothing$ unless both are $X$; a union is $X$ iff some member is $X$. All three axioms hold.
2. Every subset is open, so every "such-and-such is open" conclusion holds trivially.
3. Continuity requires $f^{-1}(U)$ open in $X$ for each open $U \subseteq Y$; everything in $X$ is open.

```tabs
tab: Lean
```lean
import Mathlib
#check @continuous_of_discreteTopology     -- every map out of a discrete space is continuous
#check @DiscreteTopology
#check @continuous_bot                      -- ⊥ is the discrete topology in Mathlib's order
```
```

> Sources: 7 Sketches, Exercise 7.29 and Solution A.7.
