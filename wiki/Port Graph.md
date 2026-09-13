#definition #example #program

For $m, n \in \mathbb{N}$, an **$(m, n)$-port graph** $(V, \mathrm{in}, \mathrm{out}, \iota)$ consists of

(i) a set $V$ of **vertices** (boxes);
(ii) functions $\mathrm{in}, \mathrm{out} : V \to \mathbb{N}$, the **in-degree** and **out-degree** (number of ports on the left/right of each box);
(iii) a bijection $\iota : \underline{m} \sqcup O \to I \sqcup \underline{n}$, where $I = \{(v, i) \mid 1 \leq i \leq \mathrm{in}(v)\}$ are the vertex inputs and $O = \{(v, i) \mid 1 \leq i \leq \mathrm{out}(v)\}$ the vertex outputs — $\iota$ says how the ports are wired: each outer input or box output is connected to exactly one box input or outer output;

subject to **acyclicity**: the *internal flow graph* with vertices $V$ and an arrow $u \to v$ whenever $\iota(u, i) = (v, j)$ has no nontrivial cycles. These are open, directed, acyclic port graphs; 7 Sketches just says "port graphs".

> Sources: 7 Sketches §5.2.2 (Definition 5.13, Example 5.14, Eq. 5.15, 5.17, Exercises 5.16, 5.18, 5.28), Definition 5.25; Kittenlab Lecture 6 ("Wiring diagrams": directed port graphs as ACSets on the schema $\mathsf{DPG}$, with boxes, ports and wires as tables); Catlab `WiringDiagram`.

**Example 5.14.** A $(2, 3)$-port graph with $V = \{a, b, c\}$, $\mathrm{in}(a) = 1$, $\mathrm{out}(a) = 3$, …; $\iota$ is a bijection between the 8 sources ($2$ outer inputs + $6$ box outputs) and the 8 targets ($5$ box inputs + $3$ outer outputs), drawn as wires.

## The prop $\mathbf{PG}$

Port graphs are the morphisms of a [[Prop]] $\mathbf{PG}$: the composite of an $(m,n)$- and an $(n,p)$-port graph is $(V \sqcup V', [\mathrm{in}, \mathrm{in}'], [\mathrm{out}, \mathrm{out}'], \iota'')$, where $\iota''$ follows $\iota$ and, if it lands on an outer output in $\underline{n}$, continues with $\iota'$ — visually "sticking them end to end, connecting the wires in order, removing the two outer boxes and adding a new one" ([[7S Chapter 5 Exercises#Exercise 5.16|7S Exercise 5.16]]). The identity on $n$ is $(\varnothing, !, !, \mathrm{id}_n)$: $n$ parallel wires. The monoidal product stacks port graphs: $G + G' = (V \sqcup V', [\mathrm{in},\mathrm{in}'], [\mathrm{out},\mathrm{out}'], \iota \sqcup \iota')$ (Eq. 5.17, [[7S Chapter 5 Exercises#Exercise 5.18|7S Exercise 5.18]]).

$\mathbf{PG}$ is the [[Free Prop]] on the signature with exactly one generator $\rho_{m,n}$ of every arity ([[7S Chapter 5 Exercises#Exercise 5.28|7S Exercise 5.28]]); a $G$-labeled port graph (a labelling $\ell : V \to G$ with matching arities) is a morphism of $\mathrm{Free}(G)$. Port graphs are thus the combinatorial form of [[Wiring Diagram|wiring diagrams]] for props, and [[Signal Flow Graph|signal flow graphs]] are port graphs labelled by the icons of $G_R$.

## Kittenlab: directed port graphs as ACSets

A **directed port graph** is a [[C-Set]] on the schema $\mathsf{DPG}$ with objects Box, InPort, OutPort, Wire and morphisms $\mathrm{box_{in}} : \mathrm{InPort} \to \mathrm{Box}$, $\mathrm{box_{out}} : \mathrm{OutPort} \to \mathrm{Box}$, $\mathrm{src} : \mathrm{Wire} \to \mathrm{OutPort}$, $\mathrm{tgt} : \mathrm{Wire} \to \mathrm{InPort}$; a wiring diagram adds outer ports. Kittenlab's preamble names $\mathsf{PortGraph}$, $\mathsf{CPG}$, $\mathsf{OpenCPG}$, $\mathsf{DWD}$, $\mathsf{UWD}$ for these variants. The bijection $\iota$ of 7 Sketches is the special case where every port has exactly one wire.

````tabs
tab: Julia
```julia
using Catlab, Catlab.WiringDiagrams
# Example 5.14 as a Catlab directed wiring diagram: boxes a (1→3), b (3→3), c (2→1), outer (2, 3)
d = WiringDiagram([:X, :X], [:X, :X, :X])
a = add_box!(d, Box(:a, [:X], [:X, :X, :X]))
b = add_box!(d, Box(:b, [:X, :X, :X], [:X, :X, :X]))
c = add_box!(d, Box(:c, [:X, :X], [:X]))
add_wires!(d, [
  (input_id(d), 1) => (a, 1), (input_id(d), 2) => (b, 3),
  (a, 1) => (c, 1), (a, 2) => (b, 2), (a, 3) => (b, 1),
  (b, 1) => (c, 2), (b, 2) => (output_id(d), 2), (b, 3) => (output_id(d), 3),
  (c, 1) => (output_id(d), 1)])
nboxes(d), nwires(d)                    # (3, 9): one wire per source port (2 outer inputs + 7 box outputs)
# monoidal product = stacking (Exercise 5.18); composition = end-to-end gluing when arities match
otimes(d, d)                            # a (4, 6)-port graph

```
tab: Haskell
```haskell
-- an (m, n)-port graph: boxes with arities and a bijection between source and target ports
data PortGraph = PortGraph
  { boxes  :: [(Int, Int)]                 -- (in-degree, out-degree) per vertex
  , wiring :: [(Port, Port)]               -- ι as a list of pairs (source ↦ target)
  }
data Port = OuterIn Int | OuterOut Int | BoxIn Int Int | BoxOut Int Int
```
````
