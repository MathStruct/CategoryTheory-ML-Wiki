#exercise #solution #proof

**Exercise 6.17.** Let $f : A \to C$, $g : B \to C$, $h : C \to D$ in a category with [[Coproduct]]s. Show
1. $\iota_A \mathbin{;} [f, g] = f$;
2. $\iota_B \mathbin{;} [f, g] = g$;
3. $[f, g] \mathbin{;} h = [f \mathbin{;} h, g \mathbin{;} h]$;
4. $[\iota_A, \iota_B] = \mathrm{id}_{A + B}$.

## Solution

1–2. These are exactly the two commuting triangles in the diagram defining the copairing $[f, g]$.
3. Both $[f, g] \mathbin{;} h$ and $[f \mathbin{;} h, g \mathbin{;} h]$ are morphisms $A + B \to D$ whose precompositions with $\iota_A, \iota_B$ are $f \mathbin{;} h$ and $g \mathbin{;} h$ (by 1–2). The [[Universal Property]] says such a morphism is unique, so they are equal.
4. $\mathrm{id}_{A+B}$ satisfies $\iota_A \mathbin{;} \mathrm{id} = \iota_A$ and $\iota_B \mathbin{;} \mathrm{id} = \iota_B$; by uniqueness of the copairing, $[\iota_A, \iota_B] = \mathrm{id}_{A+B}$.

```tabs
tab: Lean
```lean
import Mathlib
open CategoryTheory Limits
#check @Limits.coprod.inl_desc   -- coprod.inl ≫ coprod.desc f g = f
#check @Limits.coprod.inr_desc
#check @Limits.coprod.desc_comp  -- coprod.desc f g ≫ h = coprod.desc (f ≫ h) (g ≫ h)
#check @Limits.coprod.desc_inl_inr
```
tab: Haskell
```haskell
-- either f g . Left  == f
-- either f g . Right == g
-- h . either f g == either (h . f) (h . g)
-- either Left Right == id
```
```

> Sources: 7 Sketches, Exercise 6.17 and Solution A.6.
