#definition #example

For any [[Set]] $X$, the **identity function** $\mathrm{id}_X : X \to X$ (Kittenlab: $1_X$) is the [[Bijection|bijective]] function $\mathrm{id}_X(x) = x$.

> Sources: 7 Sketches Example 1.23, Proposition 1.70; Kittenlab Lecture 2; DaoFP §2.3.

It is the unit for [[Function Composition]]: $\mathrm{id}_X \mathbin{;} F = F = F \mathbin{;} \mathrm{id}_Y$ for any $F : X \to Y$. Identities are part of the data of any [[Category]] — DaoFP: the identity is "the arrow that does nothing", and $(\mathrm{id}_a \circ -)$ and $(- \circ \mathrm{id}_a)$ are identity operations on hom-sets ([[DaoFP Exercise 2.3.1]]). The identity on a [[Preorder]] is [[Monotone Map|monotone]]; $\mathrm{id}_P$ is monotone $(P,\leq) \to (P, \leq^{\mathrm{op}})$ iff $P$ is a [[Dagger Preorder]] (Example 1.72).

````tabs
tab: Julia
```julia
# Kittenlab Lecture 2
identity(A::𝔽) = 𝔽Mor(A, A, Dict(a => a for a in A))

# Catlab
using Catlab
id(FinSet(3))       # FinFunction([1,2,3], 3)
```
tab: Lean
```lean
#check @id           -- id : α → α
example (x : ℕ) : id x = x := rfl
#check @CategoryTheory.CategoryStruct.id   -- 𝟙 X in any category
```
tab: Haskell
```haskell
id' :: a -> a
id' x = x
-- Prelude's `id`; in Control.Category, `id :: cat a a`
```
````
