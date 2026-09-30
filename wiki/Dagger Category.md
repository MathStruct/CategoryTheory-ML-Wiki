#definition #example

A **dagger category** is a [[Category]] with an involutive, identity-on-objects functor $\dagger : \mathcal{C}^{\mathrm{op}} \to \mathcal{C}$: every $f : a \to b$ has an adjoint $f^\dagger : b \to a$ with $\mathrm{id}^\dagger = \mathrm{id}$, $(g \circ f)^\dagger = f^\dagger \circ g^\dagger$, $f^{\dagger\dagger} = f$. A morphism is *unitary* if $f^\dagger = f^{-1}$. The preorder version is a [[Dagger Preorder]] (an equivalence relation).

> Sources: 7 Sketches §1.2.3 (Definition 1.71, Example 1.72), §4.5 (compact closed categories in quantum theory), §5.2 ([[Category of Relations]]); DaoFP §5.2 ("Duality"); Fritz [arXiv:1908.07021](https://arxiv.org/abs/1908.07021) ([[A Synthetic Approach to Markov Kernels, Conditional Independence and Theorems on Sufficient Statistics|notes]]) Remark 13.10; Cockett et al. [arXiv:1910.07065](https://arxiv.org/abs/1910.07065) ([[Reverse Derivative Categories|notes]]) Definition 18, Theorems 41–42.

- **Examples**: $\mathbf{Rel}$ with the converse relation $R^\dagger$ ([[Category of Relations]], [[Feasibility Relation]]); [[Corelation|$\mathbf{Corel}$]]; finite-dimensional Hilbert spaces with the adjoint (conjugate transpose) — the setting of categorical quantum mechanics, a dagger [[Compact Closed Category]]; any [[Groupoid]] with $f^\dagger = f^{-1}$; the [[Prop]] of [[Signal Flow Graph|signal flow graphs]] $\mathbf{SFG}$ has a dagger reversing all wires.
- A dagger [[Frobenius Monoid]] with $\delta = \mu^\dagger$, $\epsilon = \eta^\dagger$ is a *dagger Frobenius algebra*; in a [[Hypergraph Category]] such as $\mathbf{Cospan}_{\mathbf{FinSet}}$ the dagger turns a cospan around.

## Daggers in learning and inference

- **Bayesian inversion is a dagger.** On the category of probability spaces $(X, p)$ and measure-preserving kernels, taken modulo [[Almost-Sure Equality|almost-sure equality]], sending a kernel to its [[Bayesian Inversion]] is a symmetric monoidal dagger: $(gf)^\dagger = f^\dagger g^\dagger$ and $f^{\dagger\dagger} = f$ (Fritz, Remark 13.10; Clerc, Danos, Dahlqvist & Garnier 2017).
- **Reverse mode is forward mode plus a dagger.** A [[Cartesian Differential Category]] has reverse derivatives exactly when its linear maps carry a (contextual) dagger — the transpose of the Jacobian (Cockett et al., Theorems 41–42; [[Reverse Derivative Category]]).

````tabs
tab: Julia
**Docs:** [FinRelations](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FinRelations)
```julia
using Catlab.CategoricalAlgebra.FinRelations
R = FinRelation((x, y) -> x < y, 3, 3)
Rdag = FinRelation((y, x) -> R(x, y), 3, 3)          # the converse relation R†
[Rdag(x, y) for x in 1:3, y in 1:3]                  # transpose of R's matrix
```
tab: Lean
```lean
import Mathlib
-- Mathlib has no dagger-category class; the adjoint of a linear map on inner product spaces is the key example
#check @LinearMap.adjoint
#check @ContinuousLinearMap.adjoint
```
tab: Haskell
```haskell
-- finite relations with their converse form a dagger category
type Rel a b = [(a, b)]
dagger :: Rel a b -> Rel b a
dagger = map (\(a, b) -> (b, a))
```
````
