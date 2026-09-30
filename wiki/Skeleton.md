#definition #theorem #example

A category is **skeletal** if isomorphic objects are equal; a **skeleton** of $\mathcal{C}$ is a skeletal full subcategory containing one object from each isomorphism class. Every category is [[Equivalence of Categories|equivalent]] to its skeleton (choose a representative and an iso to it for each object), so "up to equivalence" one may replace $\mathcal{C}$ by a skeleton.

> Sources: 7 Sketches Example 1.49 (identifying equivalent elements of a preorder gives a partial order), §5.2 (the prop $\mathbf{FinSet}$ has objects $\mathbb{N}$: the skeleton of finite sets); DaoFP §11.5 (equality vs. isomorphism); Kittenlab Lecture 1 (`FinSet(n)` as canonical finite sets); CTfS Definitions 4.3.4.8, 4.3.4.10, Proposition 4.3.4.9, Exercises 4.3.4.5, 4.3.4.11

- **Elections** (CTfS Definition 4.3.4.8): choosing for every isomorphism class an *elected* object $s_E$, and for every $c$ a pair of inverse isomorphisms $c \cong s_E$, yields the elected skeleton, and the proof that $\mathrm{Skel}_E(\mathcal C) \simeq \mathcal C$ sends $g : c \to c'$ to $j_{c'} \circ g \circ i_c$ between the representatives.
- The skeleton of $\mathbf{FinSet}$ is the category of ordinals $\underline{n} = \{1, \dots, n\}$ ([[Cardinality]] is a complete invariant, [[Category of Finite Sets]]); this is the [[Prop]] $\mathbf{FinSet}$ and the [[Category of Finite Sets as a Prop]].
- The skeleton of a [[Preorder]] is the [[Partial Order]] obtained by quotienting [[Equivalent Elements of a Preorder]].
- The skeleton of a [[Groupoid]] is a disjoint union of groups; of $\mathbf{FinVect}_k$, the matrices $\mathrm{Mat}(k)$ ([[Prop of Matrices]]).

````tabs
tab: Julia
**Docs:** [FinSets](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FinSets) — Kittenlab [Lecture 1](https://algebraicjulia.github.io/Kittenlab.jl/lecture1.html)
```julia
using Catlab
# Catlab's FinSet(n) already works in the skeleton of FinSet: objects are natural numbers
FinSet(3) == FinSet(3)                 # isomorphic finite sets are literally equal here
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
#check @CategoryTheory.Skeleton              -- the skeleton of a category
#check @CategoryTheory.skeletonEquivalence   -- Skeleton C ≌ C
#check @CategoryTheory.FintypeCat.Skeleton   -- ℕ with functions Fin m → Fin n
```
````
