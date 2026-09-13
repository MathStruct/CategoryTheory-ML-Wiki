#example

On the [[Natural Numbers]] $\mathbb{N}$, define $n \leq m$ if $n$ **divides** $m$ without remainder, written $n \mid m$. Then $2 \mid 4$ but $2 \nmid 3$. This is a [[Partial Order]] but not a [[Total Order]]: $4 \nmid 6$ and $6 \nmid 4$.

> Sources: 7 Sketches Example 1.45, Exercises 1.46, 1.90.

The [[Hasse Diagram]] of $\{1, \dots, 10\}$ under divisibility ([[7S Chapter 1 Exercises#Exercise 1.46|7S Exercise 1.46]]):

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}[row sep=small, column sep=small]
 & & 8 & & \\
9 & 6 & 4 \arrow[u] & 10 & \\
 & 3 \arrow[ul] \arrow[u] & 2 \arrow[ul] \arrow[u] \arrow[ur] & 5 \arrow[u] & 7 \\
 & & 1 \arrow[ul] \arrow[u] \arrow[ur] \arrow[urr] &
\end{tikzcd}
\end{document}
```

The [[Meet]] of two numbers is their **greatest common divisor** and the [[Join]] is their **least common multiple** ([[7S Chapter 1 Exercises#Exercise 1.90|7S Exercise 1.90]]): $4 \wedge 6 = 2$, $4 \vee 6 = 12$. The bottom element is $1$; adding $0$ (divisible by everything) gives a top element, making $(\mathbb{N}, \mid)$ a complete lattice.

````tabs
tab: Julia
```julia
divides(n, m) = m % n == 0
gcd(4, 6), lcm(4, 6)     # (2, 12) — meet and join
```
tab: Lean
```lean
-- Mathlib: `Nat.instLattice`? No — but ℕ with divisibility is the `Associates`/`Nat` lattice:
example : 2 ∣ 4 := ⟨2, rfl⟩
#check Nat.gcd_dvd_left     -- gcd is a lower bound
#check Nat.dvd_lcm_left     -- lcm is an upper bound
#check Nat.dvd_gcd          -- gcd is the greatest lower bound
```
tab: Haskell
```haskell
newtype Div = Div Int
instance Preorder Div where
  leq (Div n) (Div m) = m `mod` n == 0
meetDiv, joinDiv :: Int -> Int -> Int
meetDiv = gcd
joinDiv = lcm
```
````
