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

> Sources: 7 Sketches Example 1.34, 1.54, 1.88, Exercise 1.7, Proposition 1.78; Kittenlab Lecture 14; DaoFP §4.1 ("Bool"); CTfS Definition 2.7.4.9, Proposition 2.7.4.10, §4.2.4.1 (the category of propositions)

- [[Meet]] is AND, [[Join]] is OR (Example 1.88). Its [[Upper Set|upper sets]] are $\varnothing \subset \{\mathsf{true}\} \subset \{\mathsf{true},\mathsf{false}\}$.
- [[Monotone Map|Monotone maps]] $P \to \mathbb{B}$ classify [[Upper Set|upper sets]] of $P$ ([[Upper Sets Classified by Maps to Bool]]); functions $X \to \mathbb{B}$ classify [[Subset|subsets]] (Kittenlab Lecture 14). In a [[Topos]] the role of $\mathbb{B}$ is played by the [[Subobject Classifier]] $\Omega$: $\mathbb{B}$ *is* the subobject classifier of $\mathbf{Set}$ (7 Sketches Eq. 7.14), with $\mathsf{true} : 1 \to \mathbb{B}$, and $\wedge, \vee, \neg, \Rightarrow$ are characteristic maps of subsets of $\mathbb{B} \times \mathbb{B}$ and $\mathbb{B}$ ([[Internal Logic of a Topos]]).
- With $\wedge$ as monoidal product, $(\mathbb{B}, \leq, \mathsf{true}, \wedge)$ is the [[Symmetric Monoidal Preorder|symmetric monoidal preorder]] $\mathbf{Bool}$, the base of enrichment for preorders ([[Enriched Category]]); it is a [[Quantale]].
- **Category Theory for Scientists** calls $\{\mathit{True}, \mathit{False}\}$ with $\mathit{True} : \{\star\} \to \Omega$ *the* subobject classifier $\Omega$ of $\mathbf{Set}$: $\mathbf{Set}(B, \Omega) \cong \mathcal{P}(B)$ via characteristic functions, which is why $|\mathcal{P}(B)| = 2^{|B|}$ (CTfS Proposition 2.7.4.10). The complement of a subset has characteristic function $\neg \circ \chi$ ([[CTfS Chapter 2 Exercises#Exercise 2.7.4.12|CTfS Exercise 2.7.4.12]]).
- **Propositions** (CTfS §4.2.4.1): ordering statements by implication gives a preorder $\mathbf{Prop}$ in which the meet is "and", the join is "or", $\mathsf{FALSE}$ is the [[Initial Object]] and $\mathsf{TRUE}$ the [[Terminal Object]] (CTfS Example 4.5.3.9); $\mathbb{B}$ is its shadow when every statement is decided.
- DaoFP: `Bool` is the [[Sum Type]] `1 + 1` — the [[Coproduct]] of two [[Terminal Object|terminal objects]]; its two [[Global Element|global elements]] are `True` and `False`, and a function `Bool -> A` is a pair of elements of `A`.

````tabs
tab: Julia
**Docs:** [ThThinCategory (GATlab)](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/#GATlab.Stdlib.StdTheories.ThThinCategory) · [Theories & presentations](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/theories/) — Kittenlab [Lecture 14](https://algebraicjulia.github.io/Kittenlab.jl/lecture14.html)
```julia
# Bool is a preorder in Julia already: false <= true
false <= true            # true
min(true, false)         # meet = AND
max(true, false)         # join = OR
```
Catlab version (run in a fresh Julia session — Catlab exports its own `compose`, `id`, `FinFunction`, …):
```julia
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
