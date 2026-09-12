#definition #example #program

A **preorder relation** on a [[Set]] $X$ is a binary [[Relation]] on $X$, written with infix $\leq$, such that

(a) $x \leq x$ for all $x$ — **reflexivity**;
(b) if $x \leq y$ and $y \leq z$ then $x \leq z$ — **transitivity**.

A **preorder** (short for *preordered set*) is a pair $(X, \leq)$. If $x \leq y$ and $y \leq x$ we write $x \cong y$ and say $x, y$ are [[Equivalent Elements of a Preorder|equivalent]].

> Sources: 7 Sketches Definition 1.30, Remark 1.31, 1.35, 1.43; Kittenlab Lecture 5; DaoFP §10.8 ("Freyd's theorem in a preorder"), §20.1 ("Preorders" as $\mathbb{B}$-enriched categories).

## Refinements

- [[Partial Order]]: additionally $x \cong y$ implies $x = y$ (*skeletal*).
- [[Total Order]]: any two elements are comparable.
- [[Dagger Preorder]]: $x \leq y$ implies $y \leq x$; these are exactly [[Equivalence Relation|equivalence relations]].
- A preorder is an equivalence relation minus symmetry (Remark 1.31).

## Examples

[[Discrete Preorder]], [[Codiscrete Preorder]], the [[Booleans]] $\mathsf{false} \leq \mathsf{true}$, the [[Natural Numbers]] (usual, reverse, or [[Divisibility Order|divisibility]] order), the [[Real Numbers]], the [[Power Set]] under inclusion, the [[Preorder of Partitions]] under fineness, [[Upper Set|upper sets]], the [[Product Preorder]], the [[Opposite Preorder]], the [[Tree of Life]], and any [[Hasse Diagram]]. Propositions ordered by implication form a preorder whose [[Closure Operator|closure operators]] are modal operators.

## Preorders as categories (Kittenlab Lecture 5, 7 Sketches §3.2.3)

**Theorem.** Every preorder $(X, \leq)$ gives a [[Category]] with objects $X$, exactly one morphism $x \to y$ if $x \leq y$ and none otherwise. Conversely a category with at most one morphism between any two objects ("thin") is a preorder — a morphism $x \to y$ *witnesses* $x \leq y$; reflexivity is the identity, transitivity is composition. Preorders and [[Free Category|free categories]] are two ends of a spectrum (7 Sketches §3.2.3).

[[Functor|Functors]] between preorders are exactly [[Monotone Map|monotone maps]], and $\mathbf{Preord}$ is a full subcategory of $\mathbf{Cat}$ — up to the subtlety that a preorder-as-data is only *isomorphic to*, not literally, a category (see [[Subcategory]]). [[Natural Transformation|Natural transformations]] between monotone maps $f, g : P \to Q$ exist iff $f(x) \leq g(x)$ pointwise (Kittenlab Lecture 7). The functors $\mathbf{Preord} \rightleftarrows \mathbf{Cat}$ and $\mathbf{Set} \rightleftarrows \mathbf{Preord}$ (discrete/codiscrete/underlying) are listed in [[Monotone Map]].

In enriched terms, a preorder is a [[Enriched Category|$\mathbf{Bool}$-category]] (7 Sketches §2.3.2; DaoFP §20.1): the hom-object $P(x,y) \in \mathbb{B}$ is the truth value of $x \leq y$.

## Meets, joins, adjoints

[[Meet|Meets]] and [[Join|joins]] are the preorder shadows of [[Limit|limits]] and [[Colimit|colimits]]; [[Galois Connection|Galois connections]] are the shadow of [[Adjunction|adjunctions]]. Category-theoretic concepts are best understood first in preorders, "where much of the complexity is stripped away" (7 Sketches §1.5).

````tabs
tab: Julia
```julia
# Kittenlab Lecture 5: a preorder is a type with a `leq` predicate
abstract type Preorder{T} end
# leq(p::Preorder{T}, x::T, y::T)::Bool

struct RealPreorder <: Preorder{Float64} end
leq(::RealPreorder, x::Float64, y::Float64) = x <= y

# ...and the category it generates
struct PreorderMorphism{T,P<:Preorder{T}}
  p::P; dom::T; codom::T
  function PreorderMorphism(p::Preorder{T}, dom::T, codom::T) where {T}
    @assert leq(p, dom, codom)      # a morphism exists only if dom ≤ codom
    new{T,typeof(p)}(p, dom, codom)
  end
end
struct PreorderAsCat{T,P<:Preorder{T}} <: Category{T,PreorderMorphism{T,P}}
  p::P
end
Categories.dom(::PreorderAsCat, f::PreorderMorphism) = f.dom
Categories.codom(::PreorderAsCat, f::PreorderMorphism) = f.codom
Categories.id(c::PreorderAsCat{T}, x::T) where {T} = PreorderMorphism(c.p, x, x)
Categories.compose(c::PreorderAsCat, f::PreorderMorphism, g::PreorderMorphism) =
  PreorderMorphism(c.p, f.dom, g.codom)

# Catlab: the GAT of preorders and a finitely presented one
using Catlab
@present Diamond(FreePreorder) begin
  (a, b, c, d)::El
  ab::Leq(a, b); ac::Leq(a, c); bd::Leq(b, d); cd::Leq(c, d)
end
```
tab: Lean
```lean
-- Mathlib's `Preorder` class: reflexive + transitive ≤ (with < derived)
example : Preorder ℕ := inferInstance
example {α : Type} [Preorder α] (x : α) : x ≤ x := le_refl x
example {α : Type} [Preorder α] {x y z : α} (h₁ : x ≤ y) (h₂ : y ≤ z) : x ≤ z := le_trans h₁ h₂
-- every preorder is a (thin) category
example {α : Type} [Preorder α] : CategoryTheory.Category α := inferInstance
-- `Preord` is the category of preorders and monotone maps
#check CategoryTheory.Preord
```
tab: Haskell
```haskell
-- a preorder as a class with a reflexive, transitive relation (laws unenforced)
class Preorder a where
  leq :: a -> a -> Bool
  -- leq x x = True; leq x y && leq y z ==> leq x z

instance Preorder Bool where
  leq False _    = True      -- false ≤ true
  leq True  True = True
  leq True  False = False

-- the thin category: a morphism x -> y is a proof-free witness that leq x y
data Leq a = Leq a a       -- invariant: leq x y
```
````
