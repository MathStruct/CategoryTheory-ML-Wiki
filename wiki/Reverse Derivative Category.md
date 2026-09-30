#definition #theorem #example

A **Cartesian reverse differential category** (CRDC) is a Cartesian left additive category ([[Cartesian Differential Category]]) with a **reverse differential combinator**

$$
\frac{f : A \to B}{R[f] : A \times B \to A},
\qquad R[f](a, \bar b) \;\approx\; \text{"}J_f(a)^\top\,\bar b\text{"},
$$

satisfying seven axioms [RD.1–7]: $R$ is additive; $R[f](a, -)$ is additive; the reverse derivatives of identities and projections are the evident injections; $R$ of a pairing is the sum of the $R$'s; the **reverse chain rule**

$$
R[f \mathbin{;} g](a, \bar c) \;=\; R[f]\bigl(a,\ R[g](f(a),\, \bar c)\bigr) \qquad [\mathrm{RD.5}];
$$

linearity in the second argument; and symmetry of mixed partials. In $\mathbf{Smooth}$, $R[f](a, \bar b) = J_f(a)^\top \bar b$: the **vector–Jacobian product** (`vjp`, `pullback`) that reverse-mode autodiff computes.

> Sources: Cockett, Cruttwell, Gallagher, Lemay, MacAdam, Plotkin & Pronk, *Reverse derivative categories* [arXiv:1910.07065](https://arxiv.org/abs/1910.07065) ([[Reverse Derivative Categories|notes]]) Definition 13, Example 14, Theorem 16, Proposition 31, Theorems 41–42; Cruttwell, Gavranović, Ghani, Wilson & Zanasi [arXiv:2103.01931](https://arxiv.org/abs/2103.01931) ([[Categorical Foundations of Gradient-Based Learning|notes]]) §2.4 (Definition 2.6 / A.5, Proposition 2.7, Examples 2.8–2.9); Wilson & Zanasi [arXiv:2101.10488](https://arxiv.org/abs/2101.10488) ([[Reverse Derivative Ascent - A Categorical Approach to Learning Boolean Circuits|notes]]).

## The theorem that makes autodiff correct

> **Proposition** (Cockett et al. Prop. 31; Cruttwell et al. Prop. 2.7). If $\mathcal C$ is a CRDC, there is a functor $R : \mathcal C \to \mathbf{Lens}(\mathcal C)$ sending $f : A \to B$ to the lens $(f, R[f]) : (A, A) \to (B, B)$.

Functoriality of $R$ *is* axiom [RD.5]: $R[f \mathbin{;} g]$ is the lens composite of $R[f]$ and $R[g]$ ([[Lens]]). So defining a reverse derivative for each primitive and composing lenses gives the reverse derivative of the whole program — "define an `rrule` per primitive and let the AD system compose them" is the statement that $R$ is a functor. Applying the [[Para Construction]] gives

$$
\mathbf{Para}(R) \;:\; \mathbf{Para}(\mathcal C) \longrightarrow \mathbf{Para}(\mathbf{Lens}(\mathcal C)),
$$

"a parametrised map plus autodiff is a [[Parametric Lens|parametric lens]]" — the starting point of [[Gradient-Based Learning with Parametric Lenses]]. Formally $R$ is a section of the lens fibration ([[Grothendieck Construction]]).

## Reverse = forward + dagger

Every CRDC is a Cartesian differential category, with $D[f]$ recovered from $R[R[f]]$ (Theorem 16). The converse fails, and Theorems 41–42 say exactly what is missing: **a CRDC is precisely a CDC whose linear maps carry a contextual linear [[Dagger Category|dagger]]** — the transpose. Reverse mode is forward mode plus the ability to transpose Jacobians.

## Examples

| CRDC | $R[f](a, \bar b)$ | used for |
|---|---|---|
| $\mathbf{Smooth}$ (Cruttwell et al. Ex. 2.8) | $J_f(a)^\top \bar b$ | backpropagation in neural networks |
| $\mathbf{Poly}_{\mathbb Z_2}$ (Ex. 2.9) | the formal reverse derivative over $\mathbb Z_2$ | learning Boolean circuits by *reverse derivative ascent* |
| $\mathbf{Poly}_R$ for a commutative ring $R$ | formal transpose of the formal derivative | symbolic differentiation |
| a dagger category with dagger biproducts | $f^\dagger \bar b$ (Cockett et al. Ex. 19) | linear models |

## Beyond reverse derivatives

Reverse differentiation is one *functorial backward pass* among several, all of them sections of lens-like fibrations:

| backward pass | lens category | chain rule |
|---|---|---|
| reverse derivative $R[f]$ | $\mathbf{Lens}(\mathcal C)$ | [RD.5] |
| [[Bayesian Inversion]] $c^\dagger_\pi$ | [[Bayesian Lens|Bayesian lenses]] | Bayes' law composes (up to a.s. equality) |
| free-energy accumulation | [[Statistical Game|statistical games]] | AutoBayes Theorem 23 |
| best response | [[Open Game|open games]] | Nash equilibria compose |

In probabilistic deep learning these stack: an amortised inversion is itself a neural network trained through $R$.

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# Reverse derivatives as lenses (f, R[f]); composing them is backpropagation.
struct RLens{F,R}; f::F; R::R; end
compose(g::RLens, f::RLens) = RLens(g.f ∘ f.f, (a, c̄) -> f.R(a, g.R(f.f(a), c̄)))   # [RD.5]
linear(W) = RLens(x -> W * x, (x, ȳ) -> W' * ȳ)
act = RLens(x -> tanh.(x), (x, ȳ) -> ȳ .* (1 .- tanh.(x) .^ 2))
W1, W2 = [1.0 2.0; -0.5 0.3], [0.7 -1.1]
net = compose(linear(W2), compose(act, linear(W1)))
x, h = [0.2, -0.4], 1e-6
grad = net.R(x, [1.0])                                   # ∇ₓ (W2 tanh(W1 x))
fd = [(net.f(x + h * e)[1] - net.f(x - h * e)[1]) / 2h for e in ([1.0, 0], [0, 1.0])]
isapprox(grad, fd; atol = 1e-6)                          # true
# linearity of R[f](a, -) [RD.2]
net.R(x, [2.0]) ≈ 2 * net.R(x, [1.0])                   # true
```
tab: Lean
```lean
import Mathlib
-- The reverse derivative is the adjoint (transpose) of the Fréchet derivative, which needs
-- an inner product: R[f](a, b̄) = (fderiv ℝ f a)† b̄.
#check @ContinuousLinearMap.adjoint     -- the dagger that turns forward mode into reverse mode
#check @gradient                        -- ∇f a, the Riesz representative of fderiv ℝ f a
```
tab: Haskell
```haskell
-- The functor R : C -> Lens(C), for C = functions on Doubles
data RLens a b = RLens { fwd :: a -> b, rev :: a -> b -> a }

(|>) :: RLens a b -> RLens b c -> RLens a c
RLens f rf |> RLens g rg = RLens (g . f) (\a dc -> rf a (rg (f a) dc))      -- [RD.5]

scale :: Double -> RLens Double Double
scale w = RLens (* w) (\_ dy -> w * dy)
sinL :: RLens Double Double
sinL = RLens sin (\x dy -> cos x * dy)
-- rev (scale 3 |> sinL) 0.5 1 == 3 * cos 1.5
```
````
