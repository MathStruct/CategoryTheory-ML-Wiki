#definition #example

For a [[Set]] $A$, the set $\mathrm{Prt}(A)$ of all [[Partition|partitions]] of $A$ is a [[Preorder]] (in fact a [[Partial Order]]) ordered by **fineness**: $P$ is **finer** than $Q$ ($P \leq Q$; $Q$ is **coarser** than $P$) if for every part $p \in P$ there is a part $q \in Q$ with $A_p \subseteq A_q$. Viewing partitions as [[Surjection|surjections]], $f : A \twoheadrightarrow P$ is finer than $g : A \twoheadrightarrow Q$ iff there is a function $h : P \to Q$ with $f \mathbin{;} h = g$.

> Sources: 7 Sketches §1.1 (Eq. 1.5), Example 1.52, 1.68, Exercises 1.6, 1.42, 1.53, 1.77, 1.103–1.106; §1.4.2.

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
A \arrow[r, "f", two heads] \arrow[dr, "g"', two heads] & P \arrow[d, "h"] \\
 & Q
\end{tikzcd}
\end{document}
```

- The **coarsest** partition has one part and corresponds to $! : A \to \{1\}$; the **finest** has singleton parts and corresponds to $\mathrm{id}_A$ ([[7S Chapter 1 Exercises#Exercise 1.53|7S Exercise 1.53]]).
- The [[Join]] $P \vee Q$ is the transitive closure of the union of the two relations — the "join of systems" from [[Generative Effect|§1.1]]. The [[Meet]] $P \wedge Q$ has parts the nonempty intersections $A_p \cap A_q$.
- $\mathrm{Prt}(\underline{4})$ has 15 elements ([[7S Chapter 1 Exercises#Exercise 1.6|7S Exercise 1.6]]); $\mathrm{Prt}(\underline{3})$ has 5, with 12 pairs $x \leq y$ ([[7S Chapter 1 Exercises#Exercise 1.42|7S Exercise 1.42]]).
- Any [[Function]] $g : S \to T$ induces a [[Galois Connection]] $g_! : \mathrm{Prt}(S) \rightleftarrows \mathrm{Prt}(T) : g^*$ ([[Pushforward and Pullback of Partitions]]); for *surjective* $g$ the right adjoint $g^*$ is just precomposition (Example 1.68).
- The connectivity observation $\Phi : \mathrm{Prt}(\{\bullet, \circ, \ast\}) \to \mathbb{B}$ is monotone ([[7S Chapter 1 Exercises#Exercise 1.77|7S Exercise 1.77]]) but does not preserve joins.

````tabs
tab: Julia
```julia
# partitions of {1..n} as surjections; fineness = existence of h with f⋅h == g
using Catlab
f = FinFunction([1,1,2,3], 3)     # (12)(3)(4)
g = FinFunction([1,1,1,2], 2)     # (123)(4)
function finer(f::FinFunction, g::FinFunction)
  # f ≤ g iff g is constant on the fibres of f
  all(g(i) == g(j) for i in dom(f), j in dom(f) if f(i) == f(j))
end
finer(f, g)   # true
```
tab: Lean
```lean
-- Mathlib: `Setoid α` is a complete lattice; `r ≤ s` iff r is finer than s
example {α : Type} : CompleteLattice (Setoid α) := inferInstance
#check @Setoid.le_def    -- r ≤ s ↔ ∀ a b, r a b → s a b
```
tab: Haskell
```haskell
import Data.List (nub, sort)
-- a partition of a finite list as a list of blocks
type Partition a = [[a]]
finer :: Eq a => Partition a -> Partition a -> Bool
finer p q = all (\blk -> any (\blk' -> all (`elem` blk') blk) q) p
```
````
