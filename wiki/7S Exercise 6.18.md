#exercise #solution #proof

**Exercise 6.18.** Suppose $\mathcal{C}$ has [[Coproduct]]s $+$ and an [[Initial Object]] $\varnothing$. Then $(\mathcal{C}, +, \varnothing)$ is a [[Symmetric Monoidal Category]]. Develop the data:
1. Show $+$ extends to a functor $\mathcal{C} \times \mathcal{C} \to \mathcal{C}$; how does it act on morphisms?
2. Show there are isomorphisms $A + \varnothing \to A$ and $\varnothing + A \to A$.
3. Write down morphisms (a) $(A + B) + C \to A + (B + C)$, (b) $A + B \to B + A$.

## Solution

1. On objects take the coproduct; on a morphism $(f, g) : (A, B) \to (C, D)$ set $f + g := [f \mathbin{;} \iota_C,\ g \mathbin{;} \iota_D] : A + B \to C + D$. Identities are preserved: $\mathrm{id}_A + \mathrm{id}_B = [\iota_A, \iota_B] = \mathrm{id}_{A+B}$ by [[7S Exercise 6.17]] (4). Composition is preserved because both $(f + g) \mathbin{;} (h + k)$ and $(f \mathbin{;} h) + (g \mathbin{;} k)$ equal $[f \mathbin{;} h \mathbin{;} \iota_E,\ g \mathbin{;} k \mathbin{;} \iota_F]$ by uniqueness of copairing.
2. Let $!_A : \varnothing \to A$ be the unique map. The copairing $[\mathrm{id}_A, !_A] : A + \varnothing \to A$ is inverse to $\iota_A$: $\iota_A \mathbin{;} [\mathrm{id}_A, !_A] = \mathrm{id}_A$, and $[\mathrm{id}_A, !_A] \mathbin{;} \iota_A = [\iota_A, !_A \mathbin{;} \iota_A] = [\iota_A, \iota_\varnothing] = \mathrm{id}_{A + \varnothing}$ (using that $!_A \mathbin{;} \iota_A$ and $\iota_\varnothing$ are both maps out of the initial object). Symmetrically for $[!_A, \mathrm{id}_A] : \varnothing + A \to A$.
3. (a) $\alpha = \big[[\iota_A,\ \iota_B \mathbin{;} \iota_{B+C}],\ \iota_C \mathbin{;} \iota_{B+C}]$ with inverse $[\iota_A \mathbin{;} \iota_{A+B},\ [\iota_B \mathbin{;} \iota_{A+B}, \iota_C]]$. (b) $\sigma = [\iota_A', \iota_B']$ where $\iota_A' : A \to B + A$, $\iota_B' : B \to B + A$ are the inclusions of the *other* coproduct; its inverse is the analogous map $B + A \to A + B$, and $\sigma \mathbin{;} \sigma^{-1} = \mathrm{id}$ by [[7S Exercise 6.17]] (3–4).

The same argument dualised shows that finite [[Product]]s give a symmetric monoidal structure ([[Cartesian Category]]).

```tabs
tab: Julia
```julia
using Catlab
f = FinFunction([1, 2], 3); g = FinFunction([1], 2)
fg = oplus(f, g)                       # f + g : 3 → 5 in the prop FinSet
collect(fg)                            # [1, 2, 4]
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
-- Mathlib packages exactly this: coproducts give a monoidal structure.
#check @CategoryTheory.monoidalOfHasFiniteCoproducts
```
tab: Haskell
```haskell
import Data.Bifunctor (bimap)          -- bimap f g :: Either a b -> Either c d
assoc :: Either (Either a b) c -> Either a (Either b c)
assoc = either (either Left (Right . Left)) (Right . Right)
swap :: Either a b -> Either b a
swap = either Right Left
```
```

> Sources: 7 Sketches, Exercise 6.18 and Solution A.6.
