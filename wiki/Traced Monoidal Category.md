#definition #example

A **traced monoidal category** is a [[Symmetric Monoidal Category]] with a family of operations

$$
\mathrm{Tr}^X_{A,B} : \mathcal{C}(A \otimes X, B \otimes X) \to \mathcal{C}(A, B)
$$

("feed the $X$ output back into the $X$ input") satisfying naturality in $A, B$, dinaturality in $X$, vanishing ($\mathrm{Tr}^I = \mathrm{id}$, $\mathrm{Tr}^{X \otimes Y} = \mathrm{Tr}^X \circ \mathrm{Tr}^Y$), superposing and yanking ($\mathrm{Tr}^X(\sigma_{X,X}) = \mathrm{id}_X$). Graphically: a wire looping from an output back to an input.

> Sources: 7 Sketches §4.4.4 ("wiring diagrams for compact closed categories": loops via cup and cap), Proposition 4.60; §5.3 (feedback in [[Signal Flow Graph|signal flow graphs]]); §6.1 (hypergraph categories allow feedback freely).

- Every [[Compact Closed Category]] is traced: $\mathrm{Tr}^X f = (\mathrm{id}_B \otimes \epsilon_X) \circ (f \otimes \mathrm{id}_{X^*}) \circ (\mathrm{id}_A \otimes \eta_X)$ using the cup and cap; conversely the *Int construction* freely completes a traced category to a compact closed one. In $\mathbf{FinVect}$ the trace of $f : X \to X$ is the usual matrix trace. [[Hypergraph Category|Hypergraph categories]] and [[Category of Relations|$\mathbf{Rel}$]] are traced.
- Traces model feedback and fixed points: in a [[Cartesian Category]] with a trace the yanking law gives a fixed-point operator (the trace of $\langle f, \mathrm{id}\rangle$), which is how recursion is interpreted in traced models of computation (Hasegawa).

````tabs
tab: Julia
```julia
using Catlab
# trace in the free compact closed category: loop the last output back via cap/cup
@present P(FreeCompactClosedCategory) begin (A, B, X)::Ob; f::Hom(A ⊗ X, B ⊗ X) end
A, B, X, f = generators(P)
tr_f = (id(A) ⊗ (dunit(X) ⋅ braid(dual(X), X))) ⋅ (f ⊗ id(dual(X))) ⋅ (id(B) ⊗ dcounit(X))   # Tr^X f : A → B
codom(tr_f)
```
tab: Lean
```lean
import Mathlib
#check @LinearMap.trace                 -- the trace of a linear map: the paradigm example
#check @Matrix.trace
```
tab: Haskell
```haskell
-- the trace in the cartesian setting is a feedback/fixed-point operator (Control.Arrow's ArrowLoop)
class Arrow a => ArrowLoop' a where
  loop :: a (b, d) (c, d) -> a b c
-- for functions: loop f b = let (c, d) = f (b, d) in c   (uses laziness to tie the knot)
```
````
