#definition

A **structured cospan** (Baez–Courser; Catlab's `StructuredCospan`) is a cospan $L(a) \to x \leftarrow L(b)$ in a category $\mathcal{X}$ of "structured objects" (graphs, Petri nets, circuits) where $L : \mathcal{A} \to \mathcal{X}$ is a functor from a category of interfaces (typically $\mathbf{FinSet} \to \mathbf{Grph}$, sending a set to the discrete graph). Structured cospans compose by pushout in $\mathcal{X}$ and form a [[Hypergraph Category]]; they generalize [[Decorated Cospan|decorated cospans]] (which decorate the apex of a cospan in $\mathcal{A}$) and cover [[Open Graph|open graphs]], open [[Petri Net|Petri nets]] and open circuits. Kittenlab Lecture 15 motivates them as "the second step" after cospan categories; Catlab's `OpenACSetTypes(T, :V)` builds the open version of any [[C-Set|ACSet]] type.

> Sources: Kittenlab Lecture 15; Catlab documentation; 7 Sketches §6.4, §6.6 (further reading: [FS18a; FS18b]).

````tabs
tab: Julia
```julia
using Catlab
const OpenGraphOb, OpenGraph = OpenACSetTypes(Graph, :V)   # L : FinSet → Graph is the discrete-graph functor
g = OpenGraph(path_graph(Graph, 2), FinFunction([1], 2), FinFunction([2], 2))
compose(g, g) |> apex |> nv     # 3
```
````
