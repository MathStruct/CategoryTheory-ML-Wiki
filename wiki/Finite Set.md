#definition #example #program

A **finite set** (in Kittenlab's Julia-based foundations) is a *list of primitive things*, where a **primitive thing** is any possible value of a Julia variable. We write curly braces around the list: $\{1,2,3,4\}$ and $\{\mathbf a,\mathbf b,4,2,2,6\}$ are finite sets.

> Sources: Kittenlab Lecture 1–2; 7 Sketches §1.2.1 (ordinals $\underline{n}$); DaoFP §1.3.

The list may contain repetitions and the order is irrelevant *from the morphism's-eye view*: the sets $\{1,2,3,3\}$ and $\{3,2,1\}$ are not equal as Julia values, but they are [[Isomorphism|isomorphic]] — any [[Function]] out of one is a function out of the other. This is the first appearance of the principle that a representation on a computer is almost never *canonical*; see [[Isomorphism]].

## Cardinality

The **cardinality** of a finite set is the number of unique elements listed in it. See [[Cardinality]] for the theorem that two finite sets are isomorphic iff they have the same cardinality, and the [[Pigeonhole Principle]].

## Skeleton: the ordinals

7 Sketches writes $\underline{n} := \{1, 2, \dots, n\}$ for the $n$-th ordinal. Up to isomorphism every finite set is some $\underline{n}$; this is the representation Catlab uses (`FinSet(n)`). The category of finite sets is [[Category of Finite Sets|FinSet]].

````tabs
tab: Julia
```julia
# Kittenlab Lecture 1: two representations, trading completeness for performance
abstract type 𝔽 end

struct Vec𝔽 <: 𝔽            # any list of Julia values
  elems::Vector{Any}
end
Base.:(∈)(a, A::Vec𝔽) = a ∈ A.elems
Base.iterate(A::Vec𝔽) = iterate(A.elems)
Base.iterate(A::Vec𝔽, k) = iterate(A.elems, k)

struct Int𝔽 <: 𝔽            # the ordinal {1,…,n}
  n::Int
end
Base.:(∈)(a, A::Int𝔽) = 1 <= a <= A.n
Base.iterate(A::Int𝔽) = iterate(1:A.n)
Base.iterate(A::Int𝔽, k) = iterate(1:A.n, k)

A = Vec𝔽([:carrots, :peas])
B = Int𝔽(3)

# Catlab: the skeletal representation
using Catlab
X = FinSet(3)          # {1,2,3}
collect(X)             # [1, 2, 3]
Y = FinSet(Set([:a, :b]))   # Catlab also allows arbitrary finite collections
```
tab: Lean
```lean
-- Mathlib: `Fin n` is the canonical n-element type; `Finset α` a finite subset
#check (Fin 3)                  -- {0, 1, 2}
#check (Finset.range 3)         -- {0, 1, 2} : Finset ℕ
example : Fintype (Fin 3) := inferInstance
#check Fintype.card              -- cardinality of a finite type
```
tab: Haskell
```haskell
import qualified Data.Set as S
-- A finite set as a list (Kittenlab-style) ...
newtype VecF = VecF [Int]
-- ... or as an ordinal {1..n}
newtype IntF = IntF Int
elems :: IntF -> [Int]
elems (IntF n) = [1..n]
-- ... or with the containers library
fs :: S.Set Char
fs = S.fromList "carrots"
```
````
