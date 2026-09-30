#definition #example

A [[Preorder]] $(P, \leq)$ is a **partial order** (a **poset**, *partially ordered set*) if additionally

(c) $x \cong y$ implies $x = y$,

i.e. $x \leq y$ and $y \leq x$ imply $x = y$ (**antisymmetry**). In category-theoretic terms this is **skeletality**: partial orders are skeletal preorders.

> Sources: 7 Sketches Remark 1.35, 1.43, Exercise 1.85, Example 1.86; Kittenlab Lecture 5, 14; CTfS Definition 3.4.1.1, Examples 3.4.1.3–3.4.1.6, Definition 3.4.1.11, Exercises 3.4.1.7–3.4.1.12, 4.2.1.13

The difference from preorders is "rather minor": every preorder becomes a partial order by identifying equivalent elements (taking the [[Quotient Set]] by $\cong$, Example 1.49 — the [[Skeleton]] of the thin category). "A partial order is like a preorder with a fancy haircut." Any [[Discrete Preorder]] is already a partial order; a [[Codiscrete Preorder]] collapses to a point.

In a partial order, [[Meet|meets]] and [[Join|joins]] are unique when they exist ([[7S Chapter 1 Exercises#Exercise 1.85|7S Exercise 1.85]]), and $p \vee p = p \wedge p = p$ (Example 1.86).

**Cliques** (CTfS Definition 3.4.1.11): a *clique* in a preorder is a subset in which every two elements satisfy $a \leq b$; a partial order is exactly a preorder whose cliques are singletons, and, as a category, one whose only isomorphisms are identities ([[CTfS Chapter 3 Exercises#Exercise 3.4.1.12|CTfS Exercise 3.4.1.12]], [[CTfS Chapter 4 Exercises#Exercise 4.2.1.13|CTfS Exercise 4.2.1.13]]). An equivalence relation on $\{1, 2\}$ relating everything is a preorder but not a partial order. CTfS's playing-card olog ("a 4 of diamonds" is a "diamond" and a "4", …) is a partial order that is not linear; taxonomies are partial orders, usually trees ([[Hasse Diagram]], [[Tree of Life]]).

**Examples.** [[Real Numbers]] $(\mathbb{R}, \leq)$ (Kittenlab), the [[Power Set]] $\mathcal{P}(X)$ and Kittenlab's poset $\mathcal{P}(X)$ of characteristic functions $\chi : X \to \mathbb{B}$ with $\chi \leq \chi'$ iff $\chi(x) = \mathsf{true} \Rightarrow \chi'(x) = \mathsf{true}$; the [[Natural Numbers]]; [[Divisibility Order]].

````tabs
tab: Julia
**Docs:** Kittenlab [Lecture 5](https://algebraicjulia.github.io/Kittenlab.jl/lecture5.html)

**Builds on:** [[Preorder]] (`Preorder`) — run that note's Julia code first.
```julia
# Kittenlab Lecture 5: the poset of subsets of a finite set
subsetof(U, A) = all(x ∈ A for x in U)
struct SubsetPreorder{T} <: Preorder{Set{T}}
  A::Set{T}
end
function leq(p::SubsetPreorder{T}, U::Set{T}, V::Set{T}) where {T}
  @assert subsetof(U, p.A) && subsetof(V, p.A)
  subsetof(U, V)
end
```
tab: Lean
```lean
example : PartialOrder ℕ := inferInstance
example {α : Type} [PartialOrder α] {x y : α} (h₁ : x ≤ y) (h₂ : y ≤ x) : x = y :=
  le_antisymm h₁ h₂
#check CategoryTheory.PartOrd   -- the category of partial orders
```
tab: Haskell
```haskell
class Preorder a => PartialOrder a
-- law: leq x y && leq y x ==> x == y
-- e.g. subsets ordered by inclusion:
import qualified Data.Set as S
instance Ord a => Preorder (S.Set a) where
  leq = S.isSubsetOf
instance Ord a => PartialOrder (S.Set a)
```
````
