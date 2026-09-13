#definition #example

A **Hasse diagram** is a [[Graph]] $G = (V, A, s, t)$ that *presents* a [[Preorder]] $(P, \leq)$: the elements of $P$ are the vertices $V$ and $v \leq w$ iff there is a [[Path in a Graph|path]] $v \to w$ in $G$. The length-0 path $v \to v$ gives reflexivity; concatenation of paths gives transitivity. Arrows are drawn upwards (an arrow from $A$ to $B$ means $A \leq B$).

> Sources: 7 Sketches §1.1.2 (Eq. 1.5), Remark 1.39, Exercises 1.40–1.42, 1.46, 1.51, 1.57, 1.63; Example 1.76.

**Example.** The five [[Partition|partitions]] of $\{\bullet, \circ, \ast\}$ ordered by coarseness:

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}[column sep=small]
 & (\bullet \circ \ast) & \\
(\bullet\circ)(\ast) \arrow[ur] & (\bullet\ast)(\circ) \arrow[u] & (\bullet)(\circ\ast) \arrow[ul] \\
 & (\bullet)(\circ)(\ast) \arrow[ul] \arrow[u] \arrow[ur] &
\end{tikzcd}
\end{document}
```

- Any graph works, even with "useless" parallel arrows and loops ([[7S Chapter 1 Exercises#Exercise 1.40|7S Exercise 1.40]]). Arrows implied by transitivity (e.g. $x \to z$ when $x \to y \to z$) may be omitted or drawn: Example 1.76's $Q$ and $R$ are the same preorder.
- A collection of points with no arrows is the Hasse diagram of a [[Discrete Preorder]] ([[7S Chapter 1 Exercises#Exercise 1.41|7S Exercise 1.41]]).
- Categorically: the preorder presented by $G$ is the [[Preorder Reflection]] of the [[Free Category]] on $G$. Conversely a [[Presentation of a Category|presented category]] adds *named* morphisms and path equations; a preorder is the case where all parallel paths are equated.

````tabs
tab: Julia
```julia
# Catlab: present a preorder from a graph (generators = arrows, all parallel paths equal)
using Catlab
@present P(FreePreorder) begin
  (a, b, c, d)::El
  ab::Leq(a, b); ac::Leq(a, c); bd::Leq(b, d); cd::Leq(c, d)
end
# A Hasse diagram as a Catlab Graph, drawn with Graphviz
g = @acset Graph begin V = 4; E = 4; src = [1,1,2,3]; tgt = [2,3,4,4] end
# using Catlab.Graphics; to_graphviz(g)
```
tab: Haskell
```haskell
-- reachability in a graph presents a preorder
reachable :: (Eq v) => Graph v e -> v -> v -> Bool
reachable g v w = go [v] []
  where
    go [] _ = False
    go (x:xs) seen
      | x == w        = True
      | x `elem` seen = go xs seen
      | otherwise     = go (xs ++ [tgt g e | e <- edges g, src g e == x]) (x:seen)
```
````
