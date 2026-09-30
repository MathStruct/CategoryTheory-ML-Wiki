#definition #example

Let $D : \mathcal{J} \to \mathcal{C}$ be a [[Diagram]]. A **cone** $(C, c_*)$ over $D$ consists of an object $C \in \mathcal{C}$ (the **apex**) and, for each $j \in \mathcal{J}$, a morphism $c_j : C \to D(j)$ (a **leg**), such that for every $f : j \to k$ in $\mathcal{J}$, $c_k = c_j \mathbin{;} D(f)$ (all triangles commute). A **morphism of cones** $(C, c_*) \to (C', c'_*)$ is $a : C \to C'$ with $c_j = a \mathbin{;} c'_j$ for all $j$. Cones over $D$ form the category $\mathrm{Cone}(D)$, whose [[Terminal Object]] is the [[Limit]] of $D$.

Equivalently (DaoFP, Kittenlab): a cone with apex $x$ is a [[Natural Transformation]] $\Delta_x \Rightarrow D$ from the [[Constant Functor]]; "the constant functor shrinks all vertices to one, so naturality squares shrink to triangles". A **cocone** is the dual: legs $D(j) \to C$, i.e. $D \Rightarrow \Delta_x$ ([[Cocone]]).

> Sources: 7 Sketches Definition 3.92, §3.5.2 (Cone$(X, Y)$, Exercise 3.91); DaoFP §9.4–9.5 ("Cospans as natural transformations", "Limits and Colimits"); Kittenlab Lecture 9, 15; CTfS §4.5.2–4.5.3 (Definitions 4.5.2.6, 4.5.3.18, Construction 4.5.3.15)

**Cones as diagrams of a bigger shape** (CTfS Definition 4.5.3.18). Adding a new initial object $-\infty$ to the indexing category gives its [[Cone Category|left cone]] $\mathcal{J}^{\triangleleft}$; a cone over $D$ is then simply a diagram $S : \mathcal{J}^{\triangleleft} \to \mathcal{C}$ restricting to $D$, and cone morphisms are natural transformations that are the identity on $D$. This is the *slice category* $\mathcal{C}_{/D}$, and the limit is its terminal object: "none shall map to $X$ and $Y$ except through me!".

**Examples.** For the discrete diagram $\{X, Y\}$, a cone is a [[Span]] $X \leftarrow C \to Y$ (an "object equipped with morphisms to $X$ and $Y$"); for a [[Cospan]] $X \to A \leftarrow Y$, a cone is a commuting square with apex $C$ (a [[Pullback]] candidate); for a parallel pair $f, g : a \rightrightarrows b$, a cone is $p : e \to a$ with $f \circ p = g \circ p$ ([[Equalizer]]). For the empty diagram a cone is just an object. In $\mathbf{Set}$, a cone with apex $1$ over $D$ is an element of $\lim D$ ([[Finite Limits in Set]]).

````tabs
tab: Julia
**Docs:** [FinSets](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FinSets) · [Limits & colimits](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.Limits) · [Free diagrams](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FreeDiagrams) — Kittenlab [Lecture 9](https://algebraicjulia.github.io/Kittenlab.jl/lecture9.html)
```julia
using Catlab
# a cone over the cospan f : X → A ← Y : g is a Multispan with legs into X and Y agreeing in A
f = FinFunction([1, 1, 2], 2); g = FinFunction([2, 1], 2)
lim = pullback(f, g)              # the limit cone
cone = Multispan(apex(lim), legs(lim))
```
tab: Lean
```lean
#check CategoryTheory.Limits.Cone          -- structure Cone F: pt, π : (const J).obj pt ⟶ F
#check CategoryTheory.Limits.ConeMorphism
#check CategoryTheory.Limits.Cocone
```
tab: Haskell
```haskell
-- a cone over a diagram with objects indexed by j: an apex type c and legs c -> d j (commutation unenforced)
newtype Cone c d = Cone (forall j. j -> (c -> d))   -- schematic: legs selected by an index
```
````
