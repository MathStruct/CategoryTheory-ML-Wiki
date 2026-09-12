#definition #example

Given [[Preorder|preorders]] $(P, \leq)$ and $(Q, \leq)$, the **product preorder** on the product set $P \times Q$ has $(p, q) \leq (p', q')$ iff $p \leq p'$ and $q \leq q'$. This is a basic example of the [[Product Category|product of categories]].

> Sources: 7 Sketches Example 1.56, Exercise 1.57; §2.4.3 (product $\mathcal{V}$-categories).

**Example** ([[7S Exercise 1.57]]): the product of $b \geq a \leq c$ with $1 \leq 2$ has six elements $(a,1) \leq (a,2), (b,1), (c,1) \leq (b,2), (c,2)$.

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}[column sep=small]
(c,2) & & (b,2) \\
(c,1) \arrow[u] & (a,2) \arrow[ul] \arrow[ur] & (b,1) \arrow[u] \\
 & (a,1) \arrow[ul] \arrow[u] \arrow[ur] &
\end{tikzcd}
\end{document}
```

The product preorder is the [[Product]] of $P$ and $Q$ in the category $\mathbf{Preord}$ (Kittenlab-style: the projections are monotone and a monotone map into $P \times Q$ is a pair of monotone maps). Meets and joins in $P \times Q$ are computed componentwise. For [[Symmetric Monoidal Preorder|monoidal preorders]], the product of two [[Enriched Category|$\mathcal{V}$-categories]] uses $\otimes$ on hom-objects (7 Sketches §2.4.3).

````tabs
tab: Julia
```julia
struct ProductPreorder{S,T,P<:Preorder{S},Q<:Preorder{T}} <: Preorder{Tuple{S,T}}
  p::P; q::Q
end
leq(pq::ProductPreorder, x, y) = leq(pq.p, x[1], y[1]) && leq(pq.q, x[2], y[2])
```
tab: Lean
```lean
-- Mathlib: `Prod.instPreorder` is exactly the componentwise order
example {α β : Type} [Preorder α] [Preorder β] (a a' : α) (b b' : β) :
    (a, b) ≤ (a', b') ↔ a ≤ a' ∧ b ≤ b' := Prod.le_def
```
tab: Haskell
```haskell
instance (Preorder a, Preorder b) => Preorder (a, b) where
  leq (p, q) (p', q') = leq p p' && leq q q'
```
````
