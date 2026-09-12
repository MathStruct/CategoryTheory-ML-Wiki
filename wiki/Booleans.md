#definition #example

The **booleans** $\mathbb{B} = \{\mathsf{false}, \mathsf{true}\}$ form a [[Preorder]] (indeed a [[Total Order]]) with $\mathsf{false} \leq \mathsf{true}$: "$A \leq B$ iff $A$ implies $B$".

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
\mathsf{true} \\ \mathsf{false} \arrow[u]
\end{tikzcd}
\end{document}
```

> Sources: 7 Sketches Example 1.34, 1.54, 1.88, Exercise 1.7, Proposition 1.78; Kittenlab Lecture 14; DaoFP §4.1 ("Bool").

- [[Meet]] is AND, [[Join]] is OR (Example 1.88). Its [[Upper Set|upper sets]] are $\varnothing \subset \{\mathsf{true}\} \subset \{\mathsf{true},\mathsf{false}\}$.
- [[Monotone Map|Monotone maps]] $P \to \mathbb{B}$ classify [[Upper Set|upper sets]] of $P$ ([[Upper Sets Classified by Maps to Bool]]); functions $X \to \mathbb{B}$ classify [[Subset|subsets]] (Kittenlab Lecture 14). In a [[Topos]] the role of $\mathbb{B}$ is played by the [[Subobject Classifier]] $\Omega$.
- With $\wedge$ as monoidal product, $(\mathbb{B}, \leq, \mathsf{true}, \wedge)$ is the [[Symmetric Monoidal Preorder|symmetric monoidal preorder]] $\mathbf{Bool}$, the base of enrichment for preorders ([[Enriched Category]]); it is a [[Quantale]].
- DaoFP: `Bool` is the [[Sum Type]] `1 + 1` — the [[Coproduct]] of two [[Terminal Object|terminal objects]]; its two [[Global Element|global elements]] are `True` and `False`, and a function `Bool -> A` is a pair of elements of `A`.

````tabs
tab: Julia
```julia
# Bool is a preorder in Julia already: false <= true
false <= true            # true
min(true, false)         # meet = AND
max(true, false)         # join = OR

# Catlab: Bool as a thin category / the base of enrichment
using Catlab
@present BoolPreorder(FreePreorder) begin
  (f, t)::El
  f_le_t::Leq(f, t)
end
```
tab: Lean
```lean
example : false ≤ true := by decide
example : LinearOrder Bool := inferInstance    -- total order
example : BooleanAlgebra Bool := inferInstance -- ∧ = meet, ∨ = join
-- Bool as a sum type: Bool ≃ Unit ⊕ Unit
example : Bool ≃ Unit ⊕ Unit := ⟨fun b => if b then .inl () else .inr (),
  fun s => s.elim (fun _ => true) (fun _ => false), by intro b; cases b <;> rfl,
  by intro s; rcases s with ⟨⟩ | ⟨⟩ <;> rfl⟩
```
tab: Haskell
```haskell
-- DaoFP §4.1: Bool is a sum of two units
data Bool' = True' | False'
-- Bool is a preorder with False <= True (Ord instance), meet = (&&), join = (||)
meetB, joinB :: Bool -> Bool -> Bool
meetB = (&&)
joinB = (||)
-- a function out of Bool is a pair of elements (the universal property of 1 + 1)
ifThenElse :: a -> a -> Bool -> a
ifThenElse t f b = if b then t else f
```
````
