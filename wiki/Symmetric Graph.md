#definition #example

A **symmetric graph** is a [[Graph]] $\mathrm{src}, \mathrm{tgt} : A \rightrightarrows V$ with an involution $\rho : A \to A$ reversing arrows:

$$
\rho \circ \rho = \mathrm{id}_A, \qquad \mathrm{src} \circ \rho = \mathrm{tgt}, \qquad \mathrm{tgt} \circ \rho = \mathrm{src}.
$$

Every arrow comes paired with its reverse, so a symmetric graph is a model of an *undirected* graph. Like graphs, symmetric graphs are the instances ([[C-Set|$\mathcal D$-sets]]) of a small indexing category $\mathcal D$: objects $A, V$, generating arrows $\mathrm{src}, \mathrm{tgt} : A \to V$ and $\rho : A \to A$, with the three equations above as [[Database Schema|path equivalences]] (CTfS Exercise 4.2.1.21). Morphisms of symmetric graphs are the [[Natural Transformation|natural transformations]] — graph homomorphisms commuting with $\rho$.

> Sources: CTfS §4.2.1.19 (Exercises 4.2.1.21–4.2.1.23); Catlab's `SymmetricGraph` (the schema `SchSymmetricGraph`, with $\rho$ called `inv`).

## Examples

- **Undirected graphs**: an undirected edge $\{x, y\}$ with $x \neq y$ becomes the pair of arrows $x \to y$, $y \to x$ swapped by $\rho$. A **loop** at $x$ can be encoded either by one arrow fixed by $\rho$ or by two arrows swapped by $\rho$ — symmetric graphs distinguish these "half-edge" and "full" loops, a subtlety plain undirected graphs hide.
- **Road networks, friendship graphs, molecules** — anywhere "adjacent to" is a symmetric relation. The graph used for the automorphism group $D_4$ in [[Endomorphism Monoid]] is the symmetric 4-cycle.
- **The 1-skeleton of a [[Simplicial Complex]]** is a symmetric graph without loops.
- **Which graphs are symmetric?** ([[CTfS Chapter 4 Exercises#Exercise 4.2.1.22|CTfS Exercise 4.2.1.22]]) Being symmetric is *structure*, a choice of $\rho$: a graph admits one iff for every pair of vertices $x, y$ there are as many arrows $x \to y$ as $y \to x$. $\mathrm{Loop}$ (one vertex, one arrow) is symmetric with $\rho = \mathrm{id}$; the doubled 4-cycle of CTfS Exercise 4.2.1.11 is symmetric; a graph with an arrow $v \to w$ but none back is not.

## Relating graphs and symmetric graphs

The inclusion $i : \mathcal C \hookrightarrow \mathcal D$ of the graph-indexing category $\mathcal C = (A \rightrightarrows V)$ into $\mathcal D$ induces, by precomposition, the forgetful functor $\Delta_i : \mathcal D\text{-}\mathbf{Set} \to \mathcal C\text{-}\mathbf{Set}$ taking a symmetric graph to its **underlying graph**, which has both directions of each edge ([[CTfS Chapter 4 Exercises#Exercise 4.2.1.23|CTfS Exercise 4.2.1.23]]c). This is a [[Data Migration Functor]]; its left adjoint $\Sigma_i$ **symmetrizes** a graph by freely adding a reverse for each arrow.

**Counting functors** ([[CTfS Chapter 4 Exercises#Exercise 4.2.1.23|CTfS Exercise 4.2.1.23]]). There are exactly 9 functors $\mathcal C \to \mathcal D$: the hom-sets of $\mathcal D$ are $\mathcal D(A, A) = \{\mathrm{id}, \rho\}$, $\mathcal D(A, V) = \{\mathrm{src}, \mathrm{tgt}\}$ (since $\rho \,\mathbin{;}\, \mathrm{src} = \mathrm{tgt}$), $\mathcal D(V, V) = \{\mathrm{id}\}$, $\mathcal D(V, A) = \varnothing$. Sending $A, V \mapsto A, V$ gives $2 \times 2 = 4$ choices for $(\mathrm{src}, \mathrm{tgt})$; $A, V \mapsto A, A$ gives another $4$; $A, V \mapsto V, V$ gives $1$; and $A, V \mapsto V, A$ gives $0$. The "reasonable" one, $\mathrm{src} \mapsto \mathrm{src}$, $\mathrm{tgt} \mapsto \mathrm{tgt}$, is $i$; another sends both to $\mathrm{src}$, and precomposition with it takes a symmetric graph to the graph with one loop per arrow at its source.

````tabs
tab: Julia
**Docs:** [C-set morphisms](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.CSets) · [Graphs](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/graphs/)
```julia
using Catlab
# Catlab's symmetric graphs: every edge is stored with its reverse, `inv` is the involution ρ
g = path_graph(SymmetricGraph, 3)            # the undirected path 1 — 2 — 3
nv(g), ne(g)                                  # (3, 4): two edges, each in both directions
all(g[g[e, :inv], :inv] == e for e in edges(g))            # ρ ∘ ρ = id
all(g[g[e, :inv], :src] == g[e, :tgt] for e in edges(g))   # src ∘ ρ = tgt
# the underlying graph Δ_i (forget ρ): the migration along i : SchGraph → SchSymmetricGraph
U = Graph(nv(g)); add_edges!(U, g[:src], g[:tgt])
ne(U)                                         # 4 directed arrows
# the symmetric 4-cycle of CTfS Exercise 4.2.1.11
length(isomorphisms(cycle_graph(SymmetricGraph, 4), cycle_graph(SymmetricGraph, 4)))  # 8
```
tab: Lean
```lean
import Mathlib
-- Mathlib's undirected graphs are symmetric irreflexive relations:
#check SimpleGraph                 -- Adj : V → V → Prop, symm, loopless
#check @SimpleGraph.Dart           -- an arrow of the associated symmetric graph
#check @SimpleGraph.Dart.symm      -- the involution ρ reversing a dart
#check @SimpleGraph.Dart.symm_symm -- ρ ∘ ρ = id
```
tab: Haskell
```haskell
-- a symmetric graph: arrows are stored with their reversal ρ
data SymGraph = SymGraph { nV :: Int, arrows :: [(Int, Int)], rho :: Int -> Int }

-- symmetrize: freely add a reverse for each arrow (the left adjoint Σ_i)
symmetrize :: Int -> [(Int, Int)] -> SymGraph
symmetrize n es = SymGraph n (es ++ map swap es) r
  where k = length es
        r j = if j < k then j + k else j - k
        swap (x, y) = (y, x)

-- laws: rho (rho j) == j;  fst (arrows !! rho j) == snd (arrows !! j)
```
````
