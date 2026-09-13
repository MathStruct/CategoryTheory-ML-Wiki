#solution #proof

**Solution to [[7S Exercise 7.7|Exercise 7.7]].**

1. Let $j = i^{-1}$ and $g := f \mathbin{;} j : A \to B'$. Since $g \mathbin{;} i = f = \mathrm{id}_A \mathbin{;} f$, the universal property gives $j' : A \to A'$ with $j' \mathbin{;} i' = \mathrm{id}_A$ and $j' \mathbin{;} f' = f \mathbin{;} j$. For the other composite: $(i' \mathbin{;} j') \mathbin{;} i' = i'$ and $(i' \mathbin{;} j') \mathbin{;} f' = i' \mathbin{;} f \mathbin{;} j = f' \mathbin{;} i \mathbin{;} j = f'$; $\mathrm{id}_{A'}$ satisfies the same two equations, so by uniqueness $i' \mathbin{;} j' = \mathrm{id}_{A'}$.
2. Given $g : X \to A$ and $h : X \to B$ with $g \mathbin{;} f = h$, the unique $r : X \to A$ with $r \mathbin{;} \mathrm{id}_A = g$ and $r \mathbin{;} f = h$ is $r = g$.

````tabs
tab: Lean
```lean
import Mathlib
open CategoryTheory Limits
#check @CategoryTheory.IsPullback.of_horiz_isIso    -- a square with iso horizontals is a pullback
#check @CategoryTheory.IsPullback.of_id_fst          -- the identity square over f is a pullback
```
````

> Sources: 7 Sketches, Exercise 7.7 and Solution A.7.
