#definition #theorem

**$\mathbf{Preord}$** is the [[Category]] whose objects are [[Preorder|preorders]] and whose morphisms are [[Monotone Map|monotone maps]]. Identities are monotone and composites of monotone maps are monotone (Proposition 1.70, [[7S Chapter 1 Exercises#Exercise 1.71|7S Exercise 1.71]]), so this is a category.

> Sources: 7 Sketches Proposition 1.70, §3.2.4; Kittenlab Lecture 5; CTfS Exercise 4.1.1.6, Example 4.1.1.7, Proposition 4.1.2.6, Exercises 4.1.2.9–4.1.2.12, Example 5.1.3.3

**Proposition (Kittenlab).** $\mathbf{Preord}$ is a full [[Subcategory]] of $\mathbf{Cat}$ — with the caveat that "preorder as a set with a relation" is only *isomorphic to* a thin category, not literally one; category theory ignores this distinction, since a subcategory is really any category with an injective functor into $\mathcal{C}$.

- $\mathbf{Preord}$ has [[Product|products]] ([[Product Preorder]]) and the [[Opposite Preorder]] gives an involution.
- [[Isomorphism|Isomorphisms]] in $\mathbf{Preord}$ are [[Isomorphism of Preorders|isomorphisms of preorders]].
- $\mathbf{Preord}$ is equivalent to the category of [[Enriched Category|$\mathbf{Bool}$-categories]] and $\mathbf{Bool}$-functors (7 Sketches §2.3.2).
- **Choosing the morphisms is part of the definition** (CTfS Example 4.1.1.7). For the preorders that have all binary [[Join|joins]] there are (at least) three reasonable categories: no morphisms but identities; all monotone maps (the *full subcategory* of $\mathbf{Preord}$); or only the join-preserving maps. The category theorist's instinct is the last one: "if we are so interested in joins, perhaps we want joins to be preserved".
- **Preorders vs. graphs** (CTfS Proposition 4.1.2.6, Exercises 4.1.2.9–4.1.2.12): drawing an arrow for each $x \leq y$ is a functor $U : \mathbf{Preord} \to \mathbf{Grph}$; reachability ("$v \leq w$ iff there is a path") is a functor $\mathrm{Im} : \mathbf{Grph} \to \mathbf{Preord}$ with $\mathrm{Im} \circ U = \mathrm{id}$, and $\mathrm{Im} \dashv U$ ([[Reflexive Transitive Closure]]). As a left adjoint $\mathrm{Im}$ preserves coproducts but not products: for $G = H = (a \to b)$ the product graph has no path $(a,a) \to (a,b)$, although $(a,a) \leq (a,b)$ in $\mathrm{Im}\,G \times \mathrm{Im}\,H$ (CTfS Example 5.1.3.3).
- Related: $\mathbf{Pos}$ / $\mathbf{PartOrd}$ (partial orders) and $\mathbf{Lin}$ (total orders); the forgetful functor $\mathbf{Preord} \to \mathbf{Set}$ has left adjoint [[Discrete Preorder]] and right adjoint [[Codiscrete Preorder]].

````tabs
tab: Lean
```lean
#check CategoryTheory.Preord           -- bundled preorders, morphisms = OrderHom
#check CategoryTheory.PartOrd
#check CategoryTheory.Lin
#check CategoryTheory.preordToCat      -- Preord ⥤ Cat (fully faithful)
```
tab: Haskell
```haskell
-- objects: types with a Preorder instance; morphisms: Monotone a b
instance Category Monotone where   -- from Control.Category
  id = Monotone id
  Monotone g . Monotone f = Monotone (g . f)
```
````
