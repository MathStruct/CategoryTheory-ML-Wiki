#example

The **natural numbers** $\mathbb{N} := \{0, 1, 2, 3, \dots\}$ form a [[Preorder]] with the usual size ordering ($0 \leq 1$, $5 \leq 100$). This is a [[Total Order]]: for all $m, n$ either $m \leq n$ or $n \leq m$, and its [[Hasse Diagram]] is a line
$$0 \to 1 \to 2 \to 3 \to \cdots$$

> Sources: 7 Sketches Example 1.9, 1.45, 1.60, 1.62; DaoFP Ch. 7 ("Natural Numbers" as an initial algebra); Kittenlab Lecture 5 (monoids).

The same set carries many orders: the [[Discrete Preorder]], the reverse ordering ($5 \leq 3 \leq 2$, "like golf"), and the [[Divisibility Order]] $n \mid m$. The [[Booleans]] map monotonically into $\mathbb{N}$ (e.g. $\mathsf{false} \mapsto 17$, $\mathsf{true} \mapsto 24$, Example 1.60), and [[Cardinality]] $|\cdot| : \mathcal{P}(X) \to \mathbb{N}$ is monotone (Example 1.62).

Other structures on $\mathbb{N}$ appearing in the wiki: $(\mathbb{N}, +, 0)$ and $(\mathbb{N}, \cdot, 1)$ are [[Monoid|monoids]] and $(\mathbb{N}, \leq, 0, +)$ a [[Symmetric Monoidal Preorder]]; $\mathbb{N}$ is a [[Rig]]; as a type, $\mathbb{N}$ is the [[Initial Algebra]] of the functor $X \mapsto 1 + X$ ([[Natural Numbers Object]], DaoFP §7.1), with `zero` and `succ` as introduction rules and recursion/induction as elimination rules.

````tabs
tab: Julia
```julia
# ℕ with its usual, reverse, and divisibility preorders (Kittenlab-style)
struct UsualOrder <: Preorder{Int} end;  leq(::UsualOrder, m, n) = m <= n
struct ReverseOrder <: Preorder{Int} end; leq(::ReverseOrder, m, n) = n <= m
struct DivOrder <: Preorder{Int} end;    leq(::DivOrder, m, n) = n % m == 0
```
tab: Lean
```lean
example : LinearOrder ℕ := inferInstance
-- ℕ as an inductive type (initial algebra of 1 + X)
#print Nat   -- inductive Nat | zero | succ (n : Nat)
#check @Nat.rec
```
tab: Haskell
```haskell
-- DaoFP Ch. 7: natural numbers as an inductive type
data Nat = Z | S Nat

-- the eliminator (recursion principle)
rec :: a -> (a -> a) -> Nat -> a
rec z _ Z     = z
rec z s (S n) = s (rec z s n)
```
````
