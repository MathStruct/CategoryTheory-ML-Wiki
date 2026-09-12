#definition #example

The **walking arrow** $\underline{\mathbf{2}}$ (DaoFP; 7 Sketches writes $\mathbf{2}$) is the [[Free Category]] on the graph $v_1 \xrightarrow{f_1} v_2$: two objects and three morphisms $\mathrm{id}_{v_1}, f_1, \mathrm{id}_{v_2}$. A [[Functor]] $\underline{\mathbf{2}} \to \mathcal{C}$ picks out a morphism of $\mathcal{C}$ ([[DaoFP Exercise 8.2.1]]); a functor $\underline{\mathbf{2}} \to \mathbf{Set}$ is a [[Function]] (7 Sketches §3.3.1: the schema of a function is "a labeled version of $\mathbf{2}$", two tables — e.g. Beatles and Instruments).

> Sources: 7 Sketches Eq. (3.8), Example 3.36, Exercises 3.37, 3.40, 3.90; DaoFP §8.1–8.2, §9.4 ("picking objects"), §20.1 (the monoidal walking arrow = [[Bool (Monoidal Preorder)|$\mathbf{Bool}$]]).

- Functors $\underline{\mathbf{2}} \to \underline{\mathbf{3}}$: six of them, determined by where the two objects go, since $\underline{\mathbf{3}}$ is a preorder (Example 3.36, [[7S Exercise 3.37]]). Functors $\bullet \to \bullet$ into $\bullet \rightrightarrows \bullet$ are *not* determined by objects ([[7S Exercise 3.40]]).
- The **walking iso** adds an arrow back; functors out of it pick isomorphisms ([[DaoFP Exercise 8.2.2]]). The discrete $\mathbf{2}$ (two objects, no arrow) indexes [[Product|products]]/[[Coproduct|coproducts]] and $\mathcal{C} \times \mathcal{C} \cong [\mathbf{2}, \mathcal{C}]$ (DaoFP §10.2).
- $\underline{\mathbf{1}} \times \underline{\mathbf{2}} \cong \underline{\mathbf{2}}$ ([[7S Exercise 3.90]]); the [[Limit]] of a diagram of shape $\underline{\mathbf{2}}$ is its first object ([[DaoFP Exercise 9.5.1]]).
- As a [[Preorder]] it is the [[Booleans]] $\mathsf{false} \leq \mathsf{true}$; enriching over it (monoidally) gives preorders.

````tabs
tab: Julia
```julia
using Catlab
@present WalkingArrow(FreeCategory) begin
  (v1, v2)::Ob
  f1::Hom(v1, v2)
end
# a functor 2 → Set is a FinFunction; as an ACSet on this schema:
@acset_type Fn(WalkingArrow)
```
tab: Lean
```lean
-- Mathlib: the walking arrow is `Fin 2` as a preorder category, or `WalkingPair` (discrete) for products
#check CategoryTheory.Limits.WalkingPair        -- discrete two objects
#check CategoryTheory.ComposableArrows          -- ComposableArrows C n : functors from Fin (n+1)
example : CategoryTheory.Category (Fin 2) := inferInstance
```
tab: Haskell
```haskell
-- a functor out of the walking arrow is a single arrow: a value of type (a -> b) with its endpoints
data Arrow' = Arrow' { src' :: String, tgt' :: String }   -- purely syntactic
```
````
