#definition #example

A [[Preorder]] $(P, \leq)$ is a **total order** if additionally

(d) for all $x, y$, either $x \leq y$ or $y \leq x$.

Two elements are **comparable** if $x \leq y$ or $y \leq x$; a total order is a preorder in which every two elements are comparable. (7 Sketches calls a *total order* what others call a *linear preorder*; a skeletal total order is a linearly ordered set.)

> Sources: 7 Sketches Remark 1.43, Examples 1.45, 1.47, 1.89, Exercises 1.46, 1.48; Remark 1.100; CTfS Definition 3.4.1.1, Example 3.4.1.6, Exercise 3.4.1.8

- The [[Natural Numbers]] and [[Real Numbers]] with the usual order are total; their Hasse diagram "looks like a line". The [[Divisibility Order]] on $\mathbb{N}$ is not (4 and 6 are incomparable). The [[Booleans]] are total.
- In a total order the [[Meet]] of a set is its infimum and the [[Join]] its supremum (Example 1.89).
- [[Galois Connection|Galois connections]] between total orders drawn as bending arrows are adjoint iff the arrows do not cross (Remark 1.100).
- The base $[0, \infty]$ of [[Lawvere Metric Space|Lawvere metric spaces]] ([[Cost]]) is a total order.
- **Finite linear orders** (CTfS Example 3.4.1.6): every linear order on a finite set of $n+1$ elements is isomorphic to $[n] = \{0 < 1 < \cdots < n\}$; there are $n!$ linear orders on $\underline n$. Finite nonempty linear orders and monotone maps form $\mathbf{FLin}$, equivalent to the [[Simplex Category]] $\Delta$. CTfS calls a total partial order a *linear order* (Definition 3.4.1.1).

````tabs
tab: Lean
```lean
example : LinearOrder ℕ := inferInstance
example {α : Type} [LinearOrder α] (x y : α) : x ≤ y ∨ y ≤ x := le_total x y
#check CategoryTheory.Lin   -- the category of linear orders
```
tab: Haskell
```haskell
-- Haskell's Ord class is (meant to be) a total order:
-- compare :: a -> a -> Ordering  with  x <= y || y <= x
isTotalOn :: Ord a => [a] -> Bool
isTotalOn xs = and [x <= y || y <= x | x <- xs, y <- xs]
```
````
