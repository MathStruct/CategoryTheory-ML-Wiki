#definition #theorem #example

A **bicategory** is like a [[2-Category]] but with composition of 1-cells associative and unital only *up to* invertible 2-cells. $\mathbf{Prof}$ has small categories as objects, [[Profunctor|profunctors]] $P : \mathcal{C}^{\mathrm{op}} \times \mathcal{D} \to \mathbf{Set}$ as 1-cells, and natural transformations (families $\alpha_{\langle a, b\rangle}$ natural in both arguments) as 2-cells. Composition is the [[Coend]] $(P \diamond Q)\langle s, t\rangle = \int^x Q\langle s, x\rangle \times P\langle x, t\rangle$; associativity holds only up to iso (Fubini and associativity of $\times$), and the identity 1-cell is the hom-functor $\mathcal{C}(-, =)$, by the [[Ninja Yoneda Lemma|ninja co-Yoneda lemma]] — again only up to iso.

> Sources: DaoFP §17.8 ("The Bicategory of Profunctors", "Monads in a bicategory", "Prearrows as monads in Prof"); 7 Sketches §4.3 ([[Category of Profunctors|$\mathcal{V}$-profunctors]] compose *strictly* when $\mathcal{V}$ is a preorder — the quantale shadow), §4.4.

**Monads in a bicategory.** In any bicategory the endo-1-cells on an object form a monoidal category (tensor = 1-cell composition, unit = identity 1-cell); a **monad** is a monoid there: 2-cells $\mu : F \circ F \to F$, $\eta : I \to F$ with the usual laws. In $\mathbf{Cat}$ this recovers ordinary [[Monad|monads]]; in $\mathbf{Prof}$ a monad is an endo-profunctor $P$ with $\mu : P \diamond P \to P$ and $\eta : \mathcal{C}(-, =) \to P$, i.e. (by co-continuity) elements of $\int_{\langle a, b\rangle, x} \mathbf{Set}(P\langle a, x\rangle \times P\langle x, b\rangle, P\langle a, b\rangle)$ and $\int_{\langle a, b\rangle} \mathbf{Set}(\mathcal{C}(a, b), P\langle a, b\rangle)$ — in Haskell a **[[Prearrow|pre-arrow]]**:
```haskell
class Profunctor p => PreArrow p where
  (>>>) :: p a x -> p x b -> p a b
  arr   :: (a -> b) -> p a b
```
An `Arrow` is a pre-arrow that is also a [[Tambara Module]].

````tabs
tab: Julia
```julia
using Catlab.CategoricalAlgebra.FinRelations
# Bool-profunctors between finite discrete categories are relations; composition is strict there
R = FinRelation((x, y) -> x < y, 3, 3); S = FinRelation((x, y) -> x == y + 1, 3, 3)
RS = compose(R, S)                              # relational composition = the coend over Bool
[RS(x, y) for x in 1:3, y in 1:3]               # Bool matrix of the composite
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
#check @CategoryTheory.Bicategory            -- associators and unitors as invertible 2-cells
#check @CategoryTheory.Bicategory.Monad      -- monads in a bicategory
```
tab: Haskell
```haskell
class Profunctor p => PreArrow p where
  (>>>) :: p a x -> p x b -> p a b
  arr   :: (a -> b) -> p a b
instance PreArrow (->) where
  f >>> g = g . f
  arr = id
```
````
