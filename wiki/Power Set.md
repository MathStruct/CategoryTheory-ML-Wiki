#definition #example

Given a [[Set]] $X$, the **power set** $\mathcal{P}(X)$ is the set of all [[Subset|subsets]] of $X$. It is ordered by inclusion, which makes it a [[Partial Order|poset]] (and from now on this is the order meant when speaking of "the power set as an ordered set").

> Sources: 7 Sketches Example 1.50, 1.87, Exercise 1.51; Kittenlab Lecture 14; CTfS Definition 2.7.4.1, Exercise 2.7.4.2, Proposition 2.7.4.10, Exercise 5.3.2.3

For $X = \{0,1,2\}$, the [[Hasse Diagram]] of $\mathcal{P}(X)$ is a cube; in general $\mathcal{P}\{1,\dots,n\}$ looks like an $n$-dimensional cube:

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}[row sep=small, column sep=small]
 & X & \\
\{0,1\} \arrow[ur] & \{0,2\} \arrow[u] & \{1,2\} \arrow[ul] \\
\{0\} \arrow[u] \arrow[ur] & \{1\} \arrow[ul] \arrow[ur] & \{2\} \arrow[ul] \arrow[u] \\
 & \varnothing \arrow[ul] \arrow[u] \arrow[ur] &
\end{tikzcd}
\end{document}
```

**Why "power" set** (CTfS Exercise 2.7.4.2): $|\mathcal P(\varnothing)| = 1$, $|\mathcal P(\{\star\})| = 2$, $|\mathcal P(\underline 6)| = 64$, and in general $|\mathcal P(B)| = 2^{|B|}$ because subsets are the same as characteristic functions $B \to \{\mathit{True}, \mathit{False}\}$ ([[Subobject Classifier]]). Downward-closed families of subsets containing all singletons are [[Simplicial Complex|simplicial complexes]].

## Properties

- In $\mathcal{P}(X)$ the [[Meet]] of $A, B$ is $A \cap B$ and the [[Join]] is $A \cup B$ (Example 1.87); the top element is $X$ and the bottom is $\varnothing$.
- $\mathcal{P}(X) \cong \mathbb{B}^X$, the set of [[Monotone Map|maps]] $X \to \mathbb{B}$; equivalently $\mathcal{P}(X)$ is the preorder of [[Upper Set|upper sets]] of the [[Discrete Preorder]] on $X$ (Exercise 1.55).
- The elements of $\mathcal{P}(X)$ have a [[Monotone Map]] $|\cdot| : \mathcal{P}(X) \to \mathbb{N}$, cardinality (Example 1.62).
- A function $f : X \to Y$ induces three adjoint monotone maps $f_! \dashv f^* \dashv f_*$ between $\mathcal{P}(X)$ and $\mathcal{P}(Y)$; see [[Direct Image, Preimage, and Dual Image]].
- $\mathcal{P}$ with singletons and unions is a [[Monad]] on $\mathbf{Set}$ whose Kleisli arrows are relations ([[Power Set Monad]]).
- $\mathcal{P}$ is a contravariant [[Functor]] $\mathbf{Set}^{\mathrm{op}} \to \mathbf{Pos}$ via preimage, and a covariant one via direct image (Kittenlab Lecture 14).

````tabs
tab: Julia
**Docs:** [FinSets](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FinSets) · [C-set morphisms](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.CSets) — Kittenlab [Lecture 14](https://algebraicjulia.github.io/Kittenlab.jl/lecture14.html)
```julia
using Catlab
X = FinSet(3)
# subobjects of a finite set form a lattice (Catlab.CategoricalAlgebra.Subobjects)
U = Subobject(X, [1,2]); V = Subobject(X, [2,3])
meet(U, V)     # {2}
join(U, V)     # {1,2,3}
top(X); bottom(X)
```
tab: Lean
```lean
-- Mathlib: `Set α` is a complete Boolean algebra under ⊆
example (α : Type) : CompleteBooleanAlgebra (Set α) := inferInstance
example (α : Type) (A B : Set α) : A ⊓ B = A ∩ B := rfl
example (α : Type) (A B : Set α) : A ⊔ B = A ∪ B := rfl
#check (Set.powerset : Set α → Set (Set α))
```
tab: Haskell
```haskell
import Data.List (subsequences)
powerset :: [a] -> [[a]]
powerset = subsequences
-- powerset [0,1,2] == [[],[0],[1],[0,1],[2],[0,2],[1,2],[0,1,2]]
```
````
