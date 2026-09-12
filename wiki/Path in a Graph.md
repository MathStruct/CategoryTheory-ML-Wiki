#definition #example #theorem

If $G$ is a [[Graph]], a **path** in $G$ is a list $e_1, \dots, e_n$ of arrows such that $t(e_i) = s(e_{i+1})$ for $i = 1, \dots, n-1$. The path goes from $s(e_1)$ to $t(e_n)$. Sequences of length $1$ are single arrows; sequences of length $0$ start and end at the same vertex $v$ without traversing any arrow.

> Sources: 7 Sketches Definition 1.36, Example 1.37, §3.2.1; Kittenlab Lecture 6, 8, 10.

**Proposition (Kittenlab).** A path $e_1,\dots,e_n$ from $a$ to $b$ and a path $e'_1,\dots,e'_m$ from $b$ to $c$ concatenate to a path $e_1,\dots,e_n,e'_1,\dots,e'_m$ from $a$ to $c$. Hence there is a category $\mathrm{Path}(G)$ — the [[Free Category]] on $G$ — with objects the vertices, morphisms the paths, composition concatenation, and identities the empty paths.

**Example (Kittenlab).** In the Romania road map, `Arad -> Sibiu -> Fagaras -> Bucharest` is a path.

- The set of length-$n$ paths in a graph $G$ is $\mathrm{Hom}(P_n, G)$ where $P_n$ is the path graph — a [[Representable Functor]] on $\mathbf{Set}^{\mathsf{Gr}}$ (Lecture 8); Catlab's homomorphism search computes these.
- For an acyclic $G$, all paths between all pairs of vertices can be computed by dynamic programming (Lecture 10), which is computing the representables $y(a)$ of $\mathrm{Path}(G)$.
- Path equations impose relations on paths: [[Presentation of a Category|presenting categories via path equations]].

````tabs
tab: Julia
```julia
# Kittenlab Lecture 10: all paths in a DAG, as a matrix of sets of edge-lists
using Catlab
const Path = Vector{Int}
function compute_paths(g::Graph)
  n = nv(g)
  P = [Set{Path}() for _ in 1:n, _ in 1:n]
  for v in vertices(g); push!(P[v,v], Int[]); end        # identity paths
  for k in 1:n, e in edges(g)
    s, t = src(g, e), tgt(g, e)
    for v in vertices(g), p in P[v, s]
      length(p) == k - 1 && push!(P[v, t], [p; e])
    end
  end
  P
end
compute_paths(path_graph(Graph, 4))

# Kittenlab src/FinCats.jl: paths as morphisms of a finitely presented category
struct FinCatMorphism{L}
  dom::L; codom::L; path::Vector{L}
end
```
tab: Lean
```lean
#check @Quiver.Path         -- inductive: nil | cons (p : Path a b) (e : b ⟶ c)
#check @Quiver.Path.comp    -- concatenation
#check @Quiver.Path.length
```
tab: Haskell
```haskell
-- a path is a list of edges whose endpoints match
type Path e = [e]

isPath :: Eq v => Graph v e -> Path e -> Bool
isPath _ [] = True
isPath g es = and (zipWith (\e e' -> tgt g e == src g e') es (tail es))

-- composition of paths is concatenation; identity is []
compPath :: Path e -> Path e -> Path e
compPath = (++)
```
````
