#definition #example

Let $V$ be a set with [[Power Set]] $\mathcal P(V)$. A subset $X \subseteq \mathcal P(V)$ is **downward closed** if $u \in X$ and $u' \subseteq u$ imply $u' \in X$, and **contains all atoms** if $\{v\} \in X$ for every $v \in V$. A **simplicial complex** is a pair $(V, X)$ with $X \subseteq \mathcal P(V)$ downward closed and containing all atoms. Elements of $X$ are **simplices**; those of cardinality $n + 1$ form the set $X_n$ of **$n$-simplices** — vertices ($X_0 \cong V$), edges ($X_1$), triangles ($X_2$), tetrahedra ($X_3$), …; the index is the *dimension*. (Strictly, $X_0 = \{\{v\} \mid v \in V\}$, which CTfS calls "just pedantry".)

> Sources: CTfS §2.7.4.3 (Definition 2.7.4.4, Example 2.7.4.5, Exercises 2.7.4.6–2.7.4.7), §5.2.3.11 (a sheaf of worldviews on a simplicial complex), Example 4.6.1.6 (simplicial sets).

## Examples

- **The $n$-simplex** $\Delta^n$ (CTfS Example 2.7.4.5): $V = \underline{n+1}$ and $X = \mathcal P(V)$ (all nonempty subsets; the empty set is harmless). $\Delta^0$ is a point, $\Delta^1$ an edge, $\Delta^2$ a filled triangle, $\Delta^3$ a solid tetrahedron.
- **A tree**: vertices $\{1,2,3,4\}$, edges $\{1,2\}, \{2,3\}, \{2,4\}$, nothing higher — a star with centre 2.
- **The boundary of a triangle** $\partial\Delta^2$ ([[CTfS Chapter 2 Exercises#Exercise 2.7.4.7|CTfS Exercise 2.7.4.7]]): $X_0 = \{\{1\}, \{2\}, \{3\}\}$, $X_1 = \{\{1,2\}, \{1,3\}, \{2,3\}\}$ and $X_2 = X_3 = \cdots = \varnothing$ — an empty triangle, drawn with edges but not filled in.
- **Shared worldviews** (CTfS §5.2.3.11): vertices are people, and a simplex is a group of people with a common ground. Assigning to each simplex the [[Olog]] of concepts its members share, with a schema morphism for each face inclusion, gives a functor from the (opposite of the) poset of simplices to schemas; via the Alexandrov topology this becomes a [[Sheaf]] of categories.

## Drawing and the geometry it stands for

Draw a dot per vertex, a segment per edge, a filled triangle per 2-simplex, and so on; downward closure guarantees that the boundary of every drawn simplex is drawn too. A simplicial complex is a *combinatorial* description of a space glued from simplices. The simplices ordered by inclusion form a [[Preorder]] (indeed a poset), and the whole complex is determined by its **face poset**.

Simplicial complexes have poor formal properties (products and quotients misbehave); the categorical replacement is the **simplicial set**, a functor $\Delta^{\mathrm{op}} \to \mathbf{Set}$ on the [[Simplex Category]], which "has excellent formal properties that simplicial complexes do not" (CTfS Example 4.6.1.6). Every simplicial complex with a chosen total order on its vertices gives a simplicial set.

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# a simplicial complex as a downward-closed family of vertex sets (CTfS Definition 2.7.4.4)
function faces(σ)                          # all nonempty subsets of σ, via bitmasks
  v = sort(collect(σ))
  [Set(v[i] for i in eachindex(v) if isodd(mask >> (i - 1))) for mask in 1:(2^length(v) - 1)]
end
closure(generators) = unique(reduce(vcat, faces.(generators)))
simplices_of_dim(X, n) = filter(s -> length(s) == n + 1, X)
∂Δ2 = closure([Set([1, 2]), Set([1, 3]), Set([2, 3])])      # the empty triangle
length.(simplices_of_dim.(Ref(∂Δ2), 0:2))                   # [3, 3, 0]
Δ3 = closure([Set(1:4)])                                     # the solid tetrahedron
length.(simplices_of_dim.(Ref(Δ3), 0:3))                     # [4, 6, 4, 1]
```
tab: Lean
```lean
import Mathlib
-- Mathlib has geometric simplicial complexes (in a real vector space) and simplicial sets;
-- an abstract complex is easily written as a downward-closed family of finsets:
structure AbstractSimplicialComplex (V : Type*) where
  faces : Set (Finset V)
  down_closed : ∀ {s t}, s ∈ faces → t ⊆ s → t.Nonempty → t ∈ faces
  atoms : ∀ v, ({v} : Finset V) ∈ faces
#check @Geometry.SimplicialComplex   -- the geometric version
#check SSet                          -- simplicial sets: functors SimplexCategoryᵒᵖ ⥤ Type
```
tab: Haskell
```haskell
import Data.List (subsequences, nub, sort)

type Simplex = [Int]                       -- a sorted list of vertices

closure :: [Simplex] -> [Simplex]          -- all nonempty faces of the given simplices
closure gens = nub [ f | g <- gens, f <- subsequences (sort g), not (null f) ]

dim :: Int -> [Simplex] -> [Simplex]
dim n = filter ((== n + 1) . length)

boundaryTriangle :: [Simplex]
boundaryTriangle = closure [[1,2], [1,3], [2,3]]
-- map (\n -> length (dim n boundaryTriangle)) [0,1,2] == [3,3,0]
```
````
