#definition #example #theorem

**$\mathbf{Grph}$** (7 Sketches: $\mathsf{Gr}\text{-}\mathbf{Inst}$) is the [[Functor Category]] $\mathbf{Set}^{\mathsf{Gr}}$ where $\mathsf{Gr}$ is the schema $\mathsf{Arrow} \rightrightarrows \mathsf{Vertex}$ (two objects, arrows $\mathsf{source}, \mathsf{target}$, no equations): its objects are [[Graph|graphs]] and its morphisms are [[Graph Homomorphism|graph homomorphisms]]. "You may find yourself back in the primordial ooze": graphs present categories, and graphs are themselves instances on a schema which is a graph.

> Sources: 7 Sketches §3.3.5 (Eq. 3.61, Example 3.63, Exercises 3.62, 3.64), §3.2.4 ("connection"); Kittenlab Lectures 6–9, 12–13; Catlab `Graph`; CTfS Examples 4.1.1.9, 4.1.2.22, 4.5.1.5, 4.5.1.21, 4.5.3.4, §4.2.1.19

- A graph as a table: Arrow table with columns source, target; Vertex table with only IDs (Eq. 3.61 = Example 1.37). Even the schema of `easySchema` is a $\mathsf{Gr}$-instance ([[7S Chapter 3 Exercises#Exercise 3.62|7S Exercise 3.62]]).
- $\mathbf{Grph}$ has [[Coproduct|coproducts]] $(G + H)(V) = G(V) + H(V)$, $(G+H)(E) = G(E) + H(E)$ (Kittenlab Lecture 8), [[Product|products]] $(G \times H)(V) = G(V) \times H(V)$, $(G \times H)(E) = G(E) \times H(E)$ with componentwise source/target (Lecture 13), [[Pushout|pushouts]] that glue graphs along a common subgraph (Lecture 9), and all other finite limits and colimits pointwise; it is a [[Topos]].
- The [[Representable Functor|representables]] $y_V$ (one vertex) and $y_E$ (one edge, two vertices) satisfy $G(V) \cong \mathrm{Hom}(y_V, G)$ and $G(E) \cong \mathrm{Hom}(y_E, G)$ ([[Yoneda Lemma]], Kittenlab Lecture 12); $\mathrm{Hom}(P_n, G)$ is the set of length-$n$ paths (Lecture 8). A three-colouring is a homomorphism into the triangle graph (Lecture 6).
- [[Data Migration Functor|Data migration]] along $\mathsf{Gr} \to \mathsf{DDS}$ turns a [[Discrete Dynamical System]] into a graph (§3.4.1). $\mathrm{Free} : \mathbf{Grph} \to \mathbf{Cat}$ is left adjoint to the underlying graph (Example 3.74).
- **Telling graphs apart with functors** (CTfS Example 4.1.2.22). The vertex set, the arrow set, the set of loops $\mathrm{Eq}(\mathrm{src}, \mathrm{tgt})$ and the set of connected components $\mathrm{Coeq}(\mathrm{src}, \mathrm{tgt})$ are four functors $\mathbf{Grph} \to \mathbf{Set}$; functors preserve isomorphisms, so if two graphs have different numbers of loops (or components, vertices, arrows) they are not isomorphic. "It is a bit like deciding whether a number is prime by checking whether it is even or its digits add up to a multiple of 3": cheap invariants transported from $\mathbf{Set}$.
- **Universal graphs** (CTfS Example 4.5.3.4): the empty graph is initial; the terminal graph is the one-vertex one-loop graph $\mathrm{Loop}$ (a homomorphism into it has no choices to make). The product $I \times J$ has vertex set $V_I \times V_J$ and arrow set $A_I \times A_J$ — for $I = (v \to w \rightrightarrows x)$ (3 vertices, 3 arrows) and $J = (q \to r \rightleftarrows s \to t)$ (4 vertices, 4 arrows) it has $3 \cdot 4 = 12$ vertices and $3 \cdot 4 = 12$ arrows, and the two projections are visible by reading only the first or second coordinate of each row of the tables (CTfS Example 4.5.1.5).
- **Spreading a graph over time** (CTfS Example 3.3.1.7): the product with the graph $0 \to 1 \to 2 \to \cdots$ has vertices $(v, n)$ and arrows $(a, n) : (\mathrm{src}\,a, n) \to (\mathrm{tgt}\,a, n + 1)$ — every transition takes one tick.
- [[Symmetric Graph|Symmetric graphs]] are functors on a slightly bigger indexing category with an arrow-reversing involution (CTfS Example 4.2.1.21).
- Related categories: [[Weighted Graph|weighted graphs]], [[Port Graph|port graphs]], [[Open Graph|open graphs]] (cospans of graphs), [[Petri Net|Petri nets]].

````tabs
tab: Julia
**Docs:** [Limits & colimits](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.Limits) · [C-set morphisms](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.CSets) · [ACSets API](https://algebraicjulia.github.io/ACSets.jl/stable/api/) · [Graphs](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/graphs/) — Kittenlab [Lecture 6](https://algebraicjulia.github.io/Kittenlab.jl/lecture6.html), [Lecture 8](https://algebraicjulia.github.io/Kittenlab.jl/lecture8.html), [Lecture 12](https://algebraicjulia.github.io/Kittenlab.jl/lecture12.html)
```julia
using Catlab
# the schema Gr and the ACSet type Graph are built in
SchGraph                                            # V, E, src, tgt
G = @acset Graph begin V = 4; E = 5; src = [1,1,1,2,2]; tgt = [2,3,3,2,3] end   # Eq. (3.61)
H = cycle_graph(Graph, 3)
coproduct(G, H) |> apex                             # disjoint union
product(G, H) |> apex                               # product graph
homomorphisms(G, H)                                 # graph homomorphisms
```
tab: Lean
```lean
-- Mathlib: `Quiver` and `Prefunctor` give the category of (multi)graphs as quivers
#check CategoryTheory.Quiv          -- the category of quivers
#check Prefunctor                   -- a graph homomorphism
```
tab: Haskell
```haskell
-- a graph homomorphism as two functions respecting src/tgt (see Graph Homomorphism)
data GraphHom v e v' e' = GraphHom { onV :: v -> v', onE :: e -> e' }
```
````
