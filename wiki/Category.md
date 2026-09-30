#definition #example

A **category** $\mathcal{C}$ consists of the following data:

1. A collection $\mathrm{Ob}(\mathcal{C})$ of **objects** ("collection": a bunch of things like a set, but possibly too large to be a set — e.g. all sets, cf. Russell's paradox)
2. For each pair of objects $X, Y \in \mathrm{Ob}(\mathcal{C})$, a set $\mathcal{C}(X, Y)$ (also $\mathrm{Hom}_{\mathcal{C}}(X,Y)$, the **hom-set**; "hom" for homomorphism) of **morphisms** (or arrows) from $X$ to $Y$, written $f : X \to Y$; $X$ is the **domain** and $Y$ the **codomain**
3. For each object $X$, an **identity morphism** $\mathrm{id}_X \in \mathcal{C}(X, X)$
4. For each triple of objects $X, Y, Z$, a **composition operation** $$\circ_{X,Y,Z} : \mathcal{C}(Y, Z) \times \mathcal{C}(X, Y) \to \mathcal{C}(X, Z)$$ written $(g, f) \mapsto g \circ f$ — or, in the *diagrammatic order* preferred by 7 Sketches and Catlab, $f \mathbin{;} g$ ("$f$ then $g$")

subject to the following axioms:

**Associativity**: For all morphisms $f : X \to Y$, $g : Y \to Z$, $h : Z \to W$, $$(h \circ g) \circ f = h \circ (g \circ f)$$

**Identity** (unitality): For all morphisms $f : X \to Y$, $$f \circ \mathrm{id}_X = f = \mathrm{id}_Y \circ f$$

> Sources: 7 Sketches Definition 3.6 (§3.2); Kittenlab Lecture 3 ("small category"); DaoFP §1–2, §8.1. The 7 Sketches motto: *structure* (objects, morphisms, composition) plus *coherence* (associativity, unitality) — "a chain of chains is itself a long chain".

---

## Examples

- [[Category of Sets]] $\mathbf{Set}$ and [[Category of Finite Sets]] $\mathbf{FinSet}$; the category $\mathbf{Mat}$ of natural numbers and matrices (Kittenlab); [[Category of Relations]].
- [[Preorder|Preorders]] (at most one morphism between any two objects) and [[Monoid|monoids]] (exactly one object) — the two "extremes" (Kittenlab Lecture 5); [[Group|groups]].
- [[Free Category|Free categories]] on a [[Graph]], and [[Presentation of a Category|finitely presented categories]] — the [[Database Schema|database schemas]] of 7 Sketches Chapter 3.
- [[Category of Preorders|$\mathbf{Preord}$]], [[Category of Graphs|$\mathbf{Grph}$]], $\mathbf{Mon}$, $\mathbf{Grp}$, $\mathbf{Top}$, $\mathbf{Meas}$, [[Category of Categories|$\mathbf{Cat}$]]; "the category of connected Riemannian manifolds of dimension at most 4" — mathematicians work with whatever category fits their purpose.
- DaoFP's "stick-figure categories": the empty category $\mathbf{0}$, the [[Terminal Category|one-object category]] $\mathbf{1}$, the [[Walking Arrow]] $\mathbf{2}$, the walking iso, [[Discrete Category|discrete categories]] (= sets).
- Constructions: [[Opposite Category]], [[Product Category]], [[Slice Category]] and coslice, [[Functor Category]], [[Category of Elements]], [[Comma Category]].
- [[Enriched Category|Enriched categories]]: categories are exactly $\mathbf{Set}$-categories (7 Sketches Remark 3.26); [[Preorders are Bool-Categories]].

## Three viewpoints

- **7 Sketches (databases).** A category is a [[Database Schema|schema]]: objects are tables, morphisms are columns/foreign keys, and path equations are business rules; the data is a [[Functor]] $\mathcal{C} \to \mathbf{Set}$.
- **Kittenlab (computation).** A **small category** has a *set* $C_0$ of objects and sets $\mathrm{Hom}_C(x,y)$; in Julia a category is a value of a type `Category{Ob,Hom}` with `dom`, `codom`, `compose`, `id` — a "middle path" where types guide dispatch but are not relied on for correctness.
- **DaoFP (programming).** Objects are types (or propositions), arrows are functions (or implications/entailments) — the Curry–Howard–Lambek correspondence. "Programming is about composition"; an object is defined by its connections ("things are defined by their relationship to the Universe"); we compare arrows for equality but objects only up to [[Isomorphism]] (equality of objects is "evil").

## Type-Theoretic Formulation

A category may be encoded as a [[Dependent Type|dependent type]] with the following signature:

$$
\begin{align*} &\mathcal{C} : \mathrm{Type} \\ &\mathrm{Hom} : \mathcal{C} \to \mathcal{C} \to \mathrm{Type} \\ &\mathrm{id} : \prod_{X : \mathcal{C}} \mathrm{Hom}(X, X) \\ &\circ : \prod_{X, Y, Z : \mathcal{C}} \mathrm{Hom}(Y, Z) \to \mathrm{Hom}(X, Y) \to \mathrm{Hom}(X, Z) \\ &\mathrm{assoc} : \prod_{X, Y, Z, W : \mathcal{C}} \prod_{f, g, h} (h \circ g) \circ f =_{\mathrm{Hom}(X,W)} h \circ (g \circ f) \\ &\mathrm{id_left} : \prod_{X, Y : \mathcal{C}} \prod_{f : \mathrm{Hom}(X, Y)} \mathrm{id}_Y \circ f =_{\mathrm{Hom}(X,Y)} f \\ &\mathrm{id_right} : \prod_{X, Y : \mathcal{C}} \prod_{f : \mathrm{Hom}(X, Y)} f \circ \mathrm{id}_X =_{\mathrm{Hom}(X,Y)} f \end{align*}
$$

---

## Commutative Diagram

The associativity and identity axioms are expressed by the commutativity of:

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
X \arrow[r, "f"] \arrow[dr, "g \circ f"'] & Y \arrow[d, "g"] \arrow[r, "h \circ g"] & W \\
 & Z \arrow[ur, "h"'] &
\end{tikzcd}
\end{document}
```

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
X \arrow[r, "f"] \arrow[dr, "f"'] \arrow[d, "\mathrm{id}_X"'] & Y \arrow[d, "\mathrm{id}_Y"] \\
X \arrow[r, "f"'] & Y
\end{tikzcd}
\end{document}
```

---

## Related

[[Functor]] (maps between categories), [[Natural Transformation]] (maps between functors), [[Isomorphism]], [[Monomorphism]], [[Epimorphism]], [[Terminal Object]], [[Initial Object]], [[Product]], [[Coproduct]], [[Limit]], [[Colimit]], [[Adjunction]], [[Yoneda Lemma]], [[Monoidal Category]], [[2-Category]]. Universal constructions: "one gives some specified shape in a category and says *find me the best solution!*" (7 Sketches §3.6).

````tabs
tab: Julia
**Docs:** [FinSets](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FinSets) · [ThCategory (GATlab)](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/#GATlab.Stdlib.StdTheories.ThCategory) · [Theories & presentations](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/theories/) — Kittenlab [Lecture 3](https://algebraicjulia.github.io/Kittenlab.jl/lecture3.html), [Lecture 5](https://algebraicjulia.github.io/Kittenlab.jl/lecture5.html)
Kittenlab's interface (a faithful copy of `src/Categories.jl` and `src/FinSets.jl`; runs on its own, and the other Kittenlab-style tabs of the vault build on it):
```julia
module Categories
export Category, dom, codom, compose, id
abstract type Category{Ob, Hom} end
function dom(c::Category{Ob,Hom}, f::Hom)::Ob where {Ob,Hom}; error("unimplemented"); end
function codom(c::Category{Ob,Hom}, f::Hom)::Ob where {Ob,Hom}; error("unimplemented"); end
function compose(c::Category{Ob,Hom}, f::Hom, g::Hom)::Hom where {Ob,Hom}; error("unimplemented"); end  # f then g
function id(c::Category{Ob,Hom}, x::Ob)::Hom where {Ob,Hom}; error("unimplemented"); end
# Laws (not enforced):
#   compose(c, f, compose(c, g, h)) == compose(c, compose(c, f, g), h)
#   compose(c, f, id(c, codom(c, f))) == f == compose(c, id(c, dom(c, f)), f)
end
using .Categories

# the category of finite sets and functions
struct FinFunction{S,T}
  dom::AbstractSet{S}; codom::AbstractSet{T}; values::Dict{S,T}
end
(f::FinFunction{S})(x::S) where {S} = f.values[x]
struct FinSetC <: Category{AbstractSet, FinFunction} end
Categories.dom(::FinSetC, f::FinFunction) = f.dom
Categories.codom(::FinSetC, f::FinFunction) = f.codom
function Categories.compose(::FinSetC, f::FinFunction{S,T}, g::FinFunction{T,R}) where {S,T,R}
  @assert f.codom == g.dom
  FinFunction(f.dom, g.codom, Dict(x => g(f(x)) for x in f.dom))
end
Categories.id(::FinSetC, X::AbstractSet{S}) where {S} = FinFunction{S,S}(X, X, Dict(x => x for x in X))

let A = Set([:x, :y, :z]), B = Set([1, 3, 4])          # (a `let`, so notes building on this one can reuse the names)
  f = FinFunction(A, B, Dict(:x => 3, :y => 1, :z => 1))
  compose(FinSetC(), id(FinSetC(), A), f).values == f.values   # unitality: true
end
```
Catlab (a separate session — Catlab exports its own `Category`, `compose`, `id`): the generalized algebraic theory of categories and a free category on generators.
```julia
using Catlab
@present C(FreeCategory) begin
  (X, Y, Z)::Ob
  f::Hom(X, Y); g::Hom(Y, Z)
end
compose(C[:f], C[:g])           # f ⋅ g : X → Z
id(C[:X]) ⋅ C[:f] == C[:f]      # unitality holds in the free category
```
tab: Lean
```lean
-- Mathlib: CategoryTheory.Category
class Category (C : Type u) extends CategoryStruct C where   -- Hom, 𝟙, ≫ (diagrammatic order)
  id_comp : ∀ {X Y : C} (f : X ⟶ Y), 𝟙 X ≫ f = f
  comp_id : ∀ {X Y : C} (f : X ⟶ Y), f ≫ 𝟙 Y = f
  assoc : ∀ {W X Y Z : C} (f : W ⟶ X) (g : X ⟶ Y) (h : Y ⟶ Z), (f ≫ g) ≫ h = f ≫ g ≫ h

open CategoryTheory in
example : Category (Type u) := inferInstance          -- Set
open CategoryTheory in
example {α : Type} [Preorder α] : Category α := inferInstance   -- a preorder as a thin category
```
tab: Haskell
```haskell
import Prelude hiding (id, (.))

class Category cat where
  id  :: cat a a
  (.) :: cat b c -> cat a b -> cat a c
  -- Laws (not enforceable):
  -- id . f = f;  f . id = f;  (h . g) . f = h . (g . f)

-- DaoFP: the category of types and functions
instance Category (->) where
  id = \x -> x
  g . f = \x -> g (f x)
```
````
