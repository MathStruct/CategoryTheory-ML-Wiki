#definition #example #program

$\mathbf{FinSet}$ is the [[Category]] whose objects are [[Finite Set|finite sets]] and whose morphisms are [[Function|functions]] between them. Kittenlab's $\mathsf{Fin}$ and Catlab's `FinSet(n)` use the skeleton: objects are natural numbers $n$ (standing for $\underline{n} = \{1, \dots, n\}$) and a morphism $n \to m$ is a function $\{1..n\} \to \{1..m\}$, stored as a vector of integers.

> Sources: 7 Sketches Definition 3.24, Example 3.29; Kittenlab Lectures 2–4 ($\mathsf{Fin}$, `FinSetC`), 8, 13 (`FinSet`/`FinFunction` as `Int`-indexed), 15; CTfS Exercise 4.3.4.5 (the skeleton), Exercise 4.1.1.8, Example 4.3.4.4 ($\mathbf{FLin} \simeq \Delta$)

- [[Cardinality]] classifies objects up to [[Isomorphism]]: $A \cong \underline{n}$ iff $|A| = n$; there are $n!$ isomorphisms between two $n$-element sets ([[7S Chapter 3 Exercises#Exercise 3.30|7S Exercise 3.30]]).
- The skeletal version is *equivalent* but not isomorphic to the category of all finite sets: choosing a bijection $p_X : X \cong \underline{n}$ for every finite $X$ gives an [[Equivalence of Categories]] $\mathbf{Fin} \simeq \{\underline n\}_{n \in \mathbb N}$ ([[CTfS Chapter 4 Exercises#Exercise 4.3.4.5|CTfS Exercise 4.3.4.5]], [[Skeleton]]). The same move for finite nonempty linear orders gives the [[Simplex Category]] $\Delta \simeq \mathbf{FLin}$ (CTfS Example 4.3.4.4).
- $\mathbf{FinSet}$ has all finite [[Limit|limits]] and [[Colimit|colimits]] ([[Product]] $\{1..nm\}$ with index arithmetic, Kittenlab Lecture 13; [[Coproduct]] $\{1..n+m\}$; [[Pushout|pushouts]] via union-find, Lecture 9; [[Coequalizer|coequalizers]]).
- The functor $\mathsf{Fin} \to \mathbf{Mat}$ sending $f : n \to m$ to the $n \times m$ 0/1 matrix with a $1$ at $(i, f(i))$ is a [[Functor]] (Kittenlab Lecture 4): identities go to identity matrices and composition to matrix multiplication.
- [[Cospan|Cospans]] in $\mathbf{FinSet}$ are [[Undirected Wiring Diagram|undirected wiring diagrams]] (Kittenlab Lecture 15, 7 Sketches §6.2.5); [[Prop|props]] have $\mathrm{Ob} = \mathbb{N}$ like the skeleton of $\mathbf{FinSet}$, and $\mathbf{FinSet}$ itself with $+$ is a prop (7 Sketches Example 5.5).

````tabs
tab: Julia
**Docs:** [FinSets](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FinSets) · [Limits & colimits](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.Limits) · [C-set morphisms](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.CSets) — Kittenlab [Lecture 2](https://algebraicjulia.github.io/Kittenlab.jl/lecture2.html), [Lecture 4](https://algebraicjulia.github.io/Kittenlab.jl/lecture4.html), [Lecture 13](https://algebraicjulia.github.io/Kittenlab.jl/lecture13.html)

**Builds on:** [[Category]] (`FinFunction`) — run that note's Julia code first.
```julia
# Kittenlab Lecture 13: skeletal finite sets
struct FinSet′; n::Int end
struct FinFunction′; dom::FinSet′; codom::FinSet′; values::Vector{Int} end
```
Catlab version (run in a fresh Julia session — Catlab exports its own `compose`, `id`, `FinFunction`, …):
```julia
# Catlab
using Catlab
f = FinFunction([2, 3, 3], 3)          # {1,2,3} → {1,2,3}
g = FinFunction([1, 1, 2], 2)
compose(f, g)                           # FinFunction([1,2,2], 2)
is_monic(f), is_epic(g)                 # (false, true)
product(FinSet(2), FinSet(3))           # limit with two projection legs
coproduct(FinSet(2), FinSet(3))         # colimit with two injection legs
```
tab: Lean
```lean
#check CategoryTheory.FintypeCat      -- the category of finite types
#check CategoryTheory.FintypeCat.Skeleton   -- the skeleton: objects are natural numbers
example : CategoryTheory.Category CategoryTheory.FintypeCat := inferInstance
```
tab: Haskell
```haskell
-- skeletal finite sets: objects Int, morphisms 0-indexed vectors
data FinFn = FinFn { domN :: Int, codN :: Int, vals :: [Int] }
compFin :: FinFn -> FinFn -> FinFn      -- f then g
compFin (FinFn n m v) (FinFn m' k w) | m == m' = FinFn n k [ w !! i | i <- v ]
idFin :: Int -> FinFn
idFin n = FinFn n n [0 .. n-1]
```
````
