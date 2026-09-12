#definition

An **interior operator** on a [[Preorder]] $Q$ is a [[Monotone Map]] $k : Q \to Q$ with $k(q) \leq q$ and $k(k(q)) \cong k(q)$ for all $q$ — the dual of a [[Closure Operator]]. Given a [[Galois Connection]] $f \dashv g$, the composite $g \mathbin{;} f : Q \to Q$ is an interior operator (7 Sketches §1.4.4, footnote 8): $f(g(q)) \leq q$ is the counit inequality. Dually to closure operators, every interior operator arises from an adjunction with its fixed points. In [[Topological Space|topology]], the interior of a subset; categorically, a [[Comonad]] on a thin category.

> Source: 7 Sketches §1.4.4 footnote; DaoFP Ch. 16 (comonads).

````tabs
tab: Lean
```lean
-- Mathlib has `ClosureOperator`; an interior operator is a closure operator on the order dual
#check ClosureOperator
example {α : Type} [PartialOrder α] (c : ClosureOperator αᵒᵈ) : αᵒᵈ → αᵒᵈ := c
```
````
