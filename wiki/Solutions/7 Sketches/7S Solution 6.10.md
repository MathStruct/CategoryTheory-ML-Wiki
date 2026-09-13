#solution #proof

**Solution to [[7S Exercise 6.10|Exercise 6.10]].**

Since $c_1$ is initial there is a unique $f : c_1 \to c_2$; since $c_2$ is initial there is a unique $g : c_2 \to c_1$. Now $c_1 \to c_1$ has a unique morphism because $c_1$ is initial, and both $\mathrm{id}_{c_1}$ and $f \mathbin{;} g$ are such morphisms, so $f \mathbin{;} g = \mathrm{id}_{c_1}$. Symmetrically $g \mathbin{;} f = \mathrm{id}_{c_2}$. Hence $f$ is an isomorphism, and it is the *unique* isomorphism between them: initial objects are unique up to unique isomorphism.

````tabs
tab: Lean
```lean
import Mathlib
open CategoryTheory Limits
#check @Limits.initialIsoIsInitial   -- IsInitial X → (⊥_ C ≅ X)
#check @Limits.IsInitial.uniqueUpToIso
```
````

> Sources: 7 Sketches, Exercise 6.10 and Solution A.6.
