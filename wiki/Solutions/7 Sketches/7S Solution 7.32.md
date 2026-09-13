#solution #proof

**Solution to [[7S Exercise 7.32|Exercise 7.32]].**

1. $Y = X \cap Y$ with $X \in \mathrm{Op}$.
2. $\varnothing = \varnothing \cap Y$. If $A_i = B_i \cap Y$ then $A_1 \cap A_2 = (B_1 \cap B_2) \cap Y$ and $\bigcup_i A_i = (\bigcup_i B_i) \cap Y$, and $B_1 \cap B_2$, $\bigcup_i B_i$ are open in $X$.
3. The preimage of $B \in \mathrm{Op}$ under the inclusion is $B \cap Y$, which is open by definition.

````tabs
tab: Lean
```lean
import Mathlib
#check @instTopologicalSpaceSubtype
#check @isOpen_induced_iff              -- IsOpen s ↔ ∃ t, IsOpen t ∧ f ⁻¹' t = s
#check @continuous_subtype_val
```
````

> Sources: 7 Sketches, Exercise 7.32 and Solution A.7.
