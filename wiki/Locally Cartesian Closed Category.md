#definition #theorem

A category $\mathcal{C}$ is **locally cartesian closed** (LCCC) if every [[Slice Category]] $\mathcal{C}/b$ is a [[Cartesian Closed Category]]. Equivalently (for $\mathcal{C}$ with a terminal object): $\mathcal{C}$ has all [[Pullback|pullbacks]] and every [[Base Change Functor]] $f^* : \mathcal{C}/a \to \mathcal{C}/b$ has a right adjoint $\Pi_f$ (it always has the left adjoint $\Sigma_f$):
$$\Sigma_f \dashv f^* \dashv \Pi_f .$$
LCCCs are the categorical models of dependent type theory, the way CCCs model the simply typed lambda calculus.

> Sources: DaoFP §11.2 ("Base-change functor": "To model dependent types, we need to impose an additional condition: we require the category to be locally cartesian closed"), §11.3–11.4; 7 Sketches §7.2.1 (every [[Topos]] is an LCCC).

- Products in $\mathcal{C}/b$ are pullbacks over $b$ ([[DaoFP Exercise 11.2.3]]); exponentials in $\mathcal{C}/b$ are $\Pi_p(p^* -)$. $\mathcal{C}/1 \cong \mathcal{C}$, so an LCCC with a terminal object is cartesian closed.
- Examples: $\mathbf{Set}$, $\mathbf{FinSet}$, every elementary [[Topos]] (presheaf categories, [[C-Set|C-sets]], sheaves). Non-examples: $\mathbf{Cat}$, $\mathbf{Top}$ (pullback does not preserve colimits there).
- In an LCCC, $f^*$ preserves colimits and exponentials (it is a left adjoint) — which is what makes substitution well behaved in type theory.

````tabs
tab: Julia
```julia
using Catlab
# FinSet is locally cartesian closed: pullbacks exist and slices FinSet/B are cartesian closed.
# The product in FinSet/B of two bundles is their pullback over B:
p = FinFunction([1, 1, 2], 2); q = FinFunction([1, 2, 2], 2)
P = pullback(p, q); ob(P)                 # FinSet(4) = 2·1 + 1·2, the fiberwise product
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
#check @CategoryTheory.Over.pullback        -- base change on slices
#check @CategoryTheory.Over.mapPullbackAdj   -- Σ_f ⊣ f^*
-- Mathlib's `Type` is locally cartesian closed; slices `Over B` are cartesian closed
example (B : Type) : CartesianClosed (Over B) := inferInstance
```
tab: Haskell
```haskell
-- Hask is (loosely) cartesian closed; "local" cartesian closure is what dependently typed
-- languages (Idris, Agda, Lean) add: function types (x : B) -> T x
```
````
