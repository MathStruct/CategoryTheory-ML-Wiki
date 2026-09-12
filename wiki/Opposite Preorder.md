#definition

Given a [[Preorder]] $(P, \leq)$, the **opposite preorder** $(P, \leq^{\mathrm{op}})$ has the same elements, with $p \leq^{\mathrm{op}} q$ iff $q \leq p$.

> Sources: 7 Sketches Example 1.58, 1.72, Exercise 1.66; DaoFP §8.1 (opposite categories), §5.2 (duality).

- The identity is monotone $P \to P^{\mathrm{op}}$ iff $P$ is a [[Dagger Preorder]] (Example 1.72).
- The principal-upper-set map $\uparrow : P^{\mathrm{op}} \to \mathcal{U}(P)$ is monotone ([[7S Exercise 1.66]]) — a preorder [[Yoneda Embedding]].
- Reversing the order swaps [[Meet|meets]] and [[Join|joins]], and swaps left and right adjoints in a [[Galois Connection]]. This is the preorder instance of the [[Opposite Category]] and of [[Duality]] ("reversing the arrows", DaoFP §3, §5.2): every statement about preorders has a dual obtained by replacing $\leq$ with $\geq$.
- The reverse ordering on $\mathbb{N}$ ("like golf") is $\mathbb{N}^{\mathrm{op}}$; the base $\mathbf{Cost} = ([0,\infty], \geq)$ of [[Lawvere Metric Space|Lawvere metric spaces]] is $[0, \infty]^{\mathrm{op}}$.

````tabs
tab: Julia
```julia
struct OppositePreorder{T,P<:Preorder{T}} <: Preorder{T}
  p::P
end
leq(op::OppositePreorder, x, y) = leq(op.p, y, x)
```
tab: Lean
```lean
-- Mathlib: `OrderDual α` (notation αᵒᵈ) reverses the order
example {α : Type} [Preorder α] (a b : α) : OrderDual.toDual a ≤ OrderDual.toDual b ↔ b ≤ a :=
  OrderDual.toDual_le_toDual
```
tab: Haskell
```haskell
newtype Op a = Op a
instance Preorder a => Preorder (Op a) where
  leq (Op x) (Op y) = leq y x
-- cf. Data.Ord.Down for Ord
```
````
