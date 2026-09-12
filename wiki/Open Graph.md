#definition #example

An **open graph** is a [[Graph]] $G$ together with an input set $I$, an output set $O$ and functions $i : I \to G(V)$, $o : O \to G(V)$ marking input and output vertices (Kittenlab Lecture 15). It is a [[Cospan]] $I \to G(V) \leftarrow O$ of finite sets whose apex is *decorated* by the graph structure on $G(V)$ — a [[Decorated Cospan]] / [[Structured Cospan]]. Open graphs are the morphisms of a [[Hypergraph Category]] whose objects are finite sets: composing $I \to G \leftarrow O$ with $O \to H \leftarrow P$ glues $G$ and $H$ along the shared vertices by [[Pushout]] in $\mathbf{Grph}$ (computed vertex-wise and edge-wise, Kittenlab Lecture 9), and the monoidal product is disjoint union.

> Sources: Kittenlab Lecture 15 ("Open graphs are the end goal": composing open graphs as the motivation for [[Cospan|cospan categories]]); 7 Sketches §6.4 (open circuits are open labelled graphs), §6.5.

Kittenlab's plan: "our goal for the next couple lectures is to learn how to compose open graphs", first via the cospan category $\mathrm{Csp}(\mathcal{C})$ and then by generalizing (structured cospans: a functor $L : \mathbf{FinSet} \to \mathbf{Grph}$ sending a set to the discrete graph, with cospans $L(I) \to G \leftarrow L(O)$ in $\mathbf{Grph}$). Open [[Petri Net|Petri nets]], open circuits and open dynamical systems follow the same pattern; Catlab's `OpenGraph` and AlgebraicPetri's `OpenPetriNet` implement them, and [[Undirected Wiring Diagram|UWDs]] with `oapply` describe how to glue several at once.

````tabs
tab: Julia
```julia
using Catlab
# Catlab: open graphs as structured cospans with feet in FinSet
const OpenGraphOb, OpenGraph = OpenACSetTypes(Graph, :V)
G = path_graph(Graph, 3)
g1 = OpenGraph(G, FinFunction([1], 3), FinFunction([3], 3))      # inputs at vertex 1, outputs at vertex 3
g2 = OpenGraph(G, FinFunction([1], 3), FinFunction([3], 3))
g12 = compose(g1, g2)                                             # glue the two paths end to end
nv(apex(g12)), ne(apex(g12))                                      # (5, 4)
```
tab: Haskell
```haskell
-- an open graph: a graph with marked input and output vertices
data OpenGraph v e = OpenGraph { graph :: Graph v e, inputs :: [v], outputs :: [v] }
-- composition glues along outputs ~ inputs (a pushout of graphs)
```
````
