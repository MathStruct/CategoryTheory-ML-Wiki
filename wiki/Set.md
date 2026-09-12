#definition #annotation

A **set** is, informally, a collection of things called **elements**. We write $x \in X$ if $x$ is an element of $X$. Repetition and order do not matter: $\{h, 1\} = \{h, h, 1, h, 1\}$.

> Sources: 7 Sketches §1.2.1 (Example 1.9); Kittenlab Lecture 1 & 3; DaoFP Preface ("Set theory").

## Important sets and notation

| Notation | Meaning |
|---|---|
| $\varnothing$ | the empty set |
| $\{1\}$, also $1$ | a one-element set |
| $\mathbb{B}$ | the [[Booleans]] $\{\mathsf{true}, \mathsf{false}\}$ |
| $\mathbb{N}$ | the [[Natural Numbers]] $0,1,2,\dots$ |
| $\underline{n}$ | the $n$-th ordinal $\{1, 2, \dots, n\}$; $\underline{0} = \varnothing$ |
| $\mathbb{Z}$, $\mathbb{R}$ | integers, reals |

## Operations on sets

- **Subset**: $X \subseteq Y$ if every element of $X$ is in $Y$. Set-builder notation $\{y \in Y \mid P(y)\}$ picks out the elements satisfying a property $P$. See [[Subset]].
- **Union** $X_1 \cup X_2$, **intersection** $X_1 \cap X_2$ of subsets of $Y$; also $\bigcup_{n \in \mathbb{N}} X_n$ and $\bigcap_{n \in \mathbb{N}} X_n$.
- **Product** $X \times Y$: the set of pairs $(x, y)$. See [[Product]].
- **Disjoint union** $X \sqcup Y$: pairs $(x, 1)$ with $x \in X$ and $(y, 2)$ with $y \in Y$. See [[Coproduct]].
- **Power set** $\mathcal{P}(X)$: the set of all subsets. See [[Power Set]].

Notation: $Z := \mathsf{foo}$ *assigns* meaning to $Z$; $Z = \mathsf{foo}$ merely asserts equality.

## Three views of "set"

1. **7 Sketches** takes sets as primitive collections; functions are special [[Relation]]s (Definition 1.22, see [[Function]]).
2. **Kittenlab** takes *Julia values* as primitive. A [[Finite Set]] is a list of primitive things; a general (possibly infinite) set is a predicate `Any -> Bool`. If the predicate is expressible in Julia the set is *computable*. There is no set of all sets, but there is a set of all computable sets.
3. **DaoFP** notes that category theory needs only "elementary" set theory, and uses sets as the model for types, while emphasizing that the *arrows* (functions) are what matter (see [[Category of Sets]]).

````tabs
tab: Julia
```julia
# Kittenlab Lecture 3: a (possibly infinite) set is a predicate on Julia values
abstract type ComputableSet end
Base.in(x, s::ComputableSet) = error("no specific definition found")

struct FiniteSet <: ComputableSet
  A::Vector{Any}
end
Base.in(x, χ::FiniteSet) = x ∈ χ.A

struct TypeSet <: ComputableSet      # any Julia type is a set
  T::Type
end
Base.in(x, χ::TypeSet) = x isa χ.T

struct IntersectionSet <: ComputableSet
  X::ComputableSet; Y::ComputableSet
end
Base.in(x, χ::IntersectionSet) = x ∈ χ.X && x ∈ χ.Y

struct UnionSet <: ComputableSet
  X::ComputableSet; Y::ComputableSet
end
Base.in(x, χ::UnionSet) = x ∈ χ.X || x ∈ χ.Y

# product and sum as predicates
product(X, Y) = z -> (z isa Tuple) && length(z) == 2 && z[1] ∈ X && z[2] ∈ Y
struct Left;  val::Any end
struct Right; val::Any end
sum(X, Y) = x -> x isa Left ? x.val ∈ X : x isa Right ? x.val ∈ Y : false
```
tab: Lean
```lean
-- In Lean/Mathlib a "set of α" is a predicate α → Prop
#check (Set : Type u → Type u)          -- def Set (α : Type u) := α → Prop
example (α : Type) (s t : Set α) : Set α := s ∪ t
example (α : Type) (s t : Set α) : Set α := s ∩ t
example (α β : Type) : Type := α × β     -- product
example (α β : Type) : Type := α ⊕ β     -- disjoint union
#check (Set.powerset : Set α → Set (Set α))
```
tab: Haskell
```haskell
import qualified Data.Set as S

-- Finite sets from containers
s1, s2 :: S.Set Int
s1 = S.fromList [1,2,3]
s2 = S.fromList [3,4]

u = S.union s1 s2               -- {1,2,3,4}
i = S.intersection s1 s2        -- {3}
p = S.cartesianProduct s1 s2    -- product
-- Disjoint union is Either; product is (,)
d :: [Either Int Int]
d = map Left (S.toList s1) ++ map Right (S.toList s2)
```
````
