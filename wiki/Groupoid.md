#definition #example

A **groupoid** is a [[Category]] in which every morphism is an [[Isomorphism]]. A one-object groupoid is a [[Group]]; a [[Codiscrete Category]] is a groupoid (exactly one iso between any two objects); the *core* of any category (keep only the isos) is a groupoid; an [[Equivalence Relation]] viewed as a category ([[Dagger Preorder]]) is a thin groupoid. The fundamental groupoid of a space has points as objects and homotopy classes of paths as morphisms.

> Sources: 7 Sketches Example 1.72 (dagger preorders = equivalence relations), §3.2.2 (groups as one-object categories); DaoFP §11.5 (HoTT: types as $\infty$-groupoids of paths); Kittenlab Lecture 2.

- In a groupoid every morphism is both mono and epi and a [[Dagger Category|dagger]] ($f^\dagger = f^{-1}$) making all morphisms unitary.
- Groupoids are the categories equivalent to disjoint unions of groups; a groupoid's [[Skeleton]] is such a disjoint union.

````tabs
tab: Julia
```julia
using Catlab
# a groupoid presented as a category with explicit inverses
@present G(FreeCategory) begin (a, b)::Ob; f::Hom(a, b); g::Hom(b, a)
  compose(f, g) == id(a); compose(g, f) == id(b) end
```
tab: Lean
```lean
import Mathlib
#check @CategoryTheory.Groupoid          -- class extending Category with inv and the two laws
#check @CategoryTheory.Groupoid.inv
#check @CategoryTheory.Core               -- the core (maximal subgroupoid) of a category
```
tab: Haskell
```haskell
-- a groupoid on a type of objects: every arrow carries its inverse
data Iso a b = Iso { to :: a -> b, from :: b -> a }
invert :: Iso a b -> Iso b a
invert (Iso f g) = Iso g f
```
````
