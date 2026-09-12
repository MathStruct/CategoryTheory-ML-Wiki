#definition #example #program

A **graph** $G = (V, A, s, t)$ consists of a [[Set]] $V$ of **vertices**, a set $A$ of **arrows** (or **edges** $E$), and two [[Function|functions]] $s, t : A \to V$, the **source** and **target** functions. If $s(a) = v$ and $t(a) = w$ we say $a$ is an arrow from $v$ to $w$. Multiple arrows between the same vertices, and loops, are allowed.

> Sources: 7 Sketches Definition 1.36, Example 1.37, Remark 1.39, §3.2.1; Kittenlab Lecture 6, 8, 10; DaoFP §8.1 (free categories).

**Example 1.37.** $V = \{1,2,3,4\}$, $A = \{a,b,c,d,e\}$ with $s, t$ given by ([[7S Exercise 1.38]]):

| arrow | source | target |
|---|---|---|
| $a$ | 1 | 2 |
| $b$ | 1 | 3 |
| $c$ | 1 | 3 |
| $d$ | 2 | 2 |
| $e$ | 2 | 3 |

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
1 \arrow[r, "a"] \arrow[d, "b"'] \arrow[dr, "c"] & 2 \arrow[d, "e"] \arrow[loop right, "d"] \\
3 & 3' & 4
\end{tikzcd}
\end{document}
```

(the two copies of $3$ are the same vertex; there are two parallel arrows $b, c : 1 \to 3$.)

## Paths and the free category

A [[Path in a Graph|path]] is a sequence of arrows with the target of each equal to the source of the next, including length $0$ paths at each vertex. There is one path $2 \to 3$ (namely $e$), none $4 \to 3$, one $4 \to 4$ (length 0), and infinitely many $1 \to 2$ (looping through $d$). Paths compose by concatenation, giving the [[Free Category]] $\mathrm{Path}(G)$ (Kittenlab Lecture 6, 7 Sketches §3.2.1) — Kittenlab: "when you hear the word compose, your eyes should light up". A graph also *presents* a [[Preorder]] by $v \leq w$ iff there is a path $v \to w$ ([[Hasse Diagram]], Remark 1.39).

## Graphs as functors (Kittenlab Lecture 6)

A graph is exactly a functor $\mathsf{Gr} \to \mathbf{Set}$, where $\mathsf{Gr}$ is the [[Free Category]] on the graph with two vertices $E, V$ and arrows $\mathrm{src}, \mathrm{tgt} : E \to V$ — a [[C-Set]] ("acset") on the schema $\mathsf{Gr}$. A [[Graph Homomorphism]] is a [[Natural Transformation]] between such functors; the [[Representable Functor|representables]] $y_V$ and $y_E$ are the one-vertex graph and the one-edge graph, and the [[Yoneda Lemma]] says $G(V) \cong \mathrm{Hom}(y_V, G)$, $G(E) \cong \mathrm{Hom}(y_E, G)$ (Lecture 12). Graphs have [[Coproduct|coproducts]] and [[Product|products]] computed vertex-wise and edge-wise (Lectures 8, 13). Graphs are the simplest [[Database Schema|database]]: a vertex table and an edge table with two foreign keys.

Related graph-like structures: [[Petri Net]], [[Port Graph]], [[Wiring Diagram]], [[Open Graph]].

````tabs
tab: Julia
```julia
# Kittenlab Lecture 6: a graph as a diagram (functor) from the schema SchGraph
const SchGraph = FinCat(Set([:E, :V]), Dict(:src => (:E, :V), :tgt => (:E, :V)))

function path_graph(n::Int)
  V, E = Set(1:n), Set(1:(n-1))
  Diagram{Symbol, FinSet, FinFunction, FinSetC}(SchGraph, FinSetC(),
    Dict(:V => V, :E => E),
    Dict(:src => FinFunction(E, V, Dict(e => e for e in E)),
         :tgt => FinFunction(E, V, Dict(e => e + 1 for e in E))))
end

# Catlab: the schema is presented with @present and graphs are ACSets on it
using Catlab
@present SchGraph(FreeSchema) begin
  V::Ob; E::Ob
  src::Hom(E, V); tgt::Hom(E, V)
end
@acset_type MyGraph(SchGraph, index=[:src, :tgt])

g = @acset Graph begin       # Catlab.Graphs.Graph is exactly this
  V = 4; E = 5
  src = [1, 1, 1, 2, 2]
  tgt = [2, 3, 3, 2, 3]
end
nv(g), ne(g)                  # (4, 5)
src(g, 4), tgt(g, 4)          # (2, 2): the loop d
path_graph(Graph, 10)         # built-in constructors
```
tab: Lean
```lean
-- Mathlib's `Quiver` is a graph with Hom-types: multi-edges and loops allowed
#check Quiver   -- class Quiver (V : Type u) where Hom : V → V → Sort v
-- the free category on a quiver
#check CategoryTheory.Paths   -- Paths V, with `Quiver.Path`
-- Kittenlab-style: a graph as two finite types with source and target
structure Graph where
  V : Type
  E : Type
  src : E → V
  tgt : E → V
```
tab: Haskell
```haskell
-- a graph as vertex set, edge set, and source/target maps (7 Sketches Def. 1.36)
data Graph v e = Graph
  { vertices :: [v]
  , edges    :: [e]
  , src      :: e -> v
  , tgt      :: e -> v
  }

example137 :: Graph Int Char
example137 = Graph [1,2,3,4] "abcde" s t
  where s 'a' = 1; s 'b' = 1; s 'c' = 1; s 'd' = 2; s 'e' = 2
        t 'a' = 2; t 'b' = 3; t 'c' = 3; t 'd' = 2; t 'e' = 3
```
````
