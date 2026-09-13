#definition #example #program

A **$\mathcal{V}$-weighted graph** is a [[Graph]] whose edges are labeled by elements of a [[Symmetric Monoidal Preorder]] $\mathcal{V}$. A **$\mathbf{Cost}$-weighted graph** (edges labeled by $w \in [0, \infty]$) describes a city with one-way roads of given length/effort; a [[Hasse Diagram]] is a $\mathbf{Bool}$-weighted graph (edges weighted $\mathsf{true}$; $\mathsf{false}$-edges are simply not drawn).

> Sources: 7 Sketches §2.3.3 (Eqs. 2.56–2.60), §2.5.3, Exercises 2.58, 2.60, 2.62, 2.63, 2.105; footnote 4.

## Presenting $\mathcal{V}$-categories

Just as a Hasse diagram presents a [[Preorder]], a $\mathbf{Cost}$-weighted graph presents a [[Lawvere Metric Space]] on its vertex set: $d(p, q)$ is the length of the shortest path from $p$ to $q$. For the graph $Y$ with edges $x \xrightarrow{3} z$, $x \xrightarrow{4} y$, $y \xrightarrow{3} x$, $z \xrightarrow{4} y$:

| $d_Y$ | $x$ | $y$ | $z$ |
|---|---|---|---|
| $x$ | 0 | 4 | 3 |
| $y$ | 3 | 0 | 6 |
| $z$ | 7 | 4 | 0 |

The **graph matrix** $M_G$ takes no thinking: $0$ on the diagonal, the edge weight where there is an edge, $\infty$ ($= \bigvee \varnothing$, the "zero" of the [[Quantale]]) where there is none:

$$
M_Y = \begin{pmatrix} 0 & 4 & 3 \\ 3 & 0 & \infty \\ \infty & 4 & 0 \end{pmatrix}.
$$

The distance matrix is obtained by [[Matrix Multiplication in a Quantale|repeated matrix multiplication]] in $\mathbf{Cost}$: $M_Y^n$ records the shortest paths using $\leq n$ edges and the powers stabilize ($M_Y^2 = M_Y^3 = d_Y$). In general, for any quantale $\mathcal{V}$, the hom-object $\mathcal{X}(x, y)$ of the presented $\mathcal{V}$-category is $\bigvee_{\text{paths } p : x \to y} \bigotimes_{e \in p} w(e)$ — e.g. union over paths of intersections of labels for $\mathcal{V} = \mathcal{P}(M)$ ([[7S Exercise 2.62]]), or max over paths of min edge label for $\mathcal{V} = (\mathbb{N} \cup \{\infty\}, \leq, \infty, \min)$ ([[7S Exercise 2.63]]).

Graph $X$ of Eq. (2.56) ($A \xrightarrow{3} C$, $B \xrightarrow{2} A$, $B \xrightarrow{5} D$, $C \xrightarrow{3} B$, $D \xrightarrow{6} C$): $M_X$ and $d_X$ are computed in [[7S Exercise 2.60]], [[7S Exercise 2.58]], [[7S Exercise 2.105]].

Categorically this is the [[Free Category|free $\mathcal{V}$-category]] on a $\mathcal{V}$-graph, and the [[Adjoint Functor Theorem for Preorders|adjoint]] to the forgetful map.

````tabs
tab: Julia
```julia
# Cost-weighted graph → distance matrix by min-plus powers
function graph_matrix(n, edges)   # edges: (src, tgt, weight)
  M = fill(Inf, n, n); for i in 1:n; M[i,i] = 0.0; end
  for (s, t, w) in edges; M[s,t] = min(M[s,t], w); end
  M
end
minplus(A, B) = [minimum(A[i,k] + B[k,j] for k in axes(A,2)) for i in axes(A,1), j in axes(B,2)]
function distances(M)
  D = M
  while true
    D2 = minplus(D, M)
    D2 == D && return D
    D = D2
  end
end
MY = graph_matrix(3, [(1,3,3.0), (1,2,4.0), (2,1,3.0), (3,2,4.0)])   # x=1, y=2, z=3
distances(MY)   # [0 4 3; 3 0 6; 7 4 0]

# Catlab: weighted graphs as ACSets with an edge attribute
using Catlab
g = @acset WeightedGraph{Float64} begin
  V = 3; E = 4; src = [1,1,2,3]; tgt = [3,2,1,2]; weight = [3.0, 4.0, 3.0, 4.0]
end
```
tab: Lean
```lean
-- Mathlib: `SimpleGraph` has `edist`/`dist` for unweighted graphs; weighted shortest paths
-- are the (min,+) closure; by hand:
def graphMatrix (n : ℕ) (w : Fin n → Fin n → ENNReal) : Matrix (Fin n) (Fin n) ENNReal :=
  fun i j => if i = j then 0 else w i j
```
tab: Haskell
```haskell
-- min-plus closure of a weighted adjacency matrix (Floyd–Warshall)
type Mat = [[Double]]
minPlus :: Mat -> Mat -> Mat
minPlus a b = [ [ minimum (zipWith (+) row col) | col <- cols ] | row <- a ]
  where cols = foldr (zipWith (:)) (repeat []) b

distances :: Mat -> Mat
distances m = go m where go d = let d' = minPlus d m in if d' == d then d else go d'
```
````
