#definition #example

A **groupoid** is a [[Category]] in which every morphism is an [[Isomorphism]]. A one-object groupoid is a [[Group]]; a [[Codiscrete Category]] is a groupoid (exactly one iso between any two objects); the *core* of any category (keep only the isos) is a groupoid; an [[Equivalence Relation]] viewed as a category ([[Dagger Preorder]]) is a thin groupoid. The fundamental groupoid of a space has points as objects and homotopy classes of paths as morphisms.

> Sources: 7 Sketches Example 1.72 (dagger preorders = equivalence relations), §3.2.2 (groups as one-object categories); DaoFP §11.5 (HoTT: types as $\infty$-groupoids of paths); Kittenlab Lecture 2; CTfS §4.2.3.6 (Definition 4.2.3.7, Application 4.2.3.9, Examples 4.2.3.10–4.2.3.11, Exercises 4.2.3.12–4.2.3.14)

- **The fundamental groupoid** $\Pi_1(X)$ (CTfS Example 4.2.3.11): objects are the points of a space $X$, morphisms $p \to q$ are paths from $p$ to $q$ up to homotopy; composition is concatenation, identities are constant paths, and every path is invertible by running it backwards. $\Pi_1 : \mathbf{Top} \to \mathbf{Grpd}$ is a functor. On a torus, $\Pi_1(T)(p, q)$ is the set of ways to get from $p$ to $q$ up to deformation, $\cong \mathbb Z^2$ (winding around the two holes, [[CTfS Chapter 4 Exercises#Exercise 4.2.3.12|CTfS Exercise 4.2.3.12]]).
- **Line integrals** (CTfS Exercises 4.1.1.10, 4.2.3.13): for a vector field $F$ on the plane, oriented curves modulo "same endpoints and same $\int_C F$" form a groupoid $\mathcal C_F$; homotopic curves have equal integrals (on an open set where $F$ is defined), so there is an identity-on-objects functor $\Pi_1(U) \to \mathcal C_F$, and $F$ is conservative iff $\mathcal C_F$ is indiscrete.
- **Materials** (CTfS Application 4.2.3.9): states of a material with a morphism $s \to s'$ whenever a physical transformation exists form a preorder; the *elastic deformation region* consists of the states that can return to the original state $s_0$, and "elastically equivalent" states form a groupoid inside it. Other examples: arithmetic expressions with justifications that two expressions have the same value (CTfS Exercise 4.2.3.14; the connected components are the integers), and Weinstein's tilings of a bathroom floor.
- In a groupoid every morphism is both mono and epi and a [[Dagger Category|dagger]] ($f^\dagger = f^{-1}$) making all morphisms unitary.
- Groupoids are the categories equivalent to disjoint unions of groups; a groupoid's [[Skeleton]] is such a disjoint union.

````tabs
tab: Julia
**Docs:** [ThCategory (GATlab)](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/#GATlab.Stdlib.StdTheories.ThCategory) · [Theories & presentations](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/theories/) — Kittenlab [Lecture 2](https://algebraicjulia.github.io/Kittenlab.jl/lecture2.html)
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
