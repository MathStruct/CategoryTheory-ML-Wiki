#exercise #solution #proof

**Exercise 7.6.** Show that in $\mathbf{Set}$, [[Monomorphism|monomorphisms]] (defined via the [[Pullback]] square of Definition 7.5) are exactly the [[Injection|injections]].

## Solution

The pullback definition says: for all $g_1, g_2 : X \to A$, if $g_1 \mathbin{;} f = g_2 \mathbin{;} f$ then $g_1 = g_2$ (the mediating arrow into the pullback $A$ must equal both $g_1$ and $g_2$).
1. If $f$ is mono and $f(a_1) = f(a_2)$, take $X = \{*\}$, $g_i(*) = a_i$. Then $g_1 \mathbin{;} f = g_2 \mathbin{;} f$, so $g_1 = g_2$, so $a_1 = a_2$.
2. If $f$ is injective and $g_1 \mathbin{;} f = g_2 \mathbin{;} f$, then for each $x$, $f(g_1(x)) = f(g_2(x))$ gives $g_1(x) = g_2(x)$; so $g_1 = g_2$.

```tabs
tab: Lean
```lean
import Mathlib
#check @CategoryTheory.mono_iff_injective
```
```

> Sources: 7 Sketches, Exercise 7.6 and Solution A.7.
