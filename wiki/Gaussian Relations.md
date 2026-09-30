#definition #theorem #example

**Gaussian relations** combine two kinds of uncertainty in one category: *probabilistic* uncertainty (Gaussian noise) and *nondeterministic* uncertainty (complete ignorance, described by linear subspaces). An **extended Gaussian distribution** on $\mathbb R^n$ (Stein & Samuelson, Definition 3) is a pair $(D, \psi)$ of a linear subspace $D \subseteq \mathbb R^n$ — the *nondeterministic fibre*, directions about which nothing is known — and a Gaussian distribution $\psi$ on the quotient $\mathbb R^n / D$. Ordinary Gaussians have $D = 0$; the **uninformative (improper) prior** has $D = \mathbb R^n$; a linear subspace with no noise has $\psi$ a point mass.

Extended Gaussian maps form a [[Markov Category]] $\mathbf{GaussEx}$ **with conditionals** (Theorem 13), and allowing *conditioning on equality* yields the category $\mathbf{GaussRel}$ of Gaussian relations, which is a [[Hypergraph Category]]: every object carries a special commutative Frobenius structure, so wires can be **merged**, not just copied. Decorated cospans of vector spaces with Gaussian decorations map onto it by a hypergraph functor (Theorem 14): *initialise every variable with an uninformative prior, condition the equations, read off the posterior*.

> Sources: Stein & Samuelson, *A Category for Unifying Gaussian Probability and Nondeterminism* (CALCO 2023) [arXiv:2204.14024](https://arxiv.org/abs/2204.14024) ([[A Category for Unifying Gaussian Probability and Nondeterminism|notes]]) Definitions 3, 5, 8, 11, Theorems 13–15, Examples 1, 2, 16; Stein, Zanasi, Piedeleu & Samuelson, *Graphical Quadratic Algebra* [arXiv:2403.02284](https://arxiv.org/abs/2403.02284) ([[Graphical Quadratic Algebra|notes]]); Bonchi, Sobociński & Zanasi, *Interacting Hopf Algebras* [arXiv:1403.7048](https://arxiv.org/abs/1403.7048) ([[Interacting Hopf Algebras|notes]]) (linear relations); Willems (2013), *Open stochastic systems*; Fritz [arXiv:1908.07021](https://arxiv.org/abs/1908.07021) ([[A Synthetic Approach to Markov Kernels, Conditional Independence and Theorems on Sufficient Statistics|notes]]) §6 ($\mathbf{Gauss}$).

## Why it is the right home for linear-Gaussian inference

- **Improper priors are first-class.** An uninformative prior is not a limit or a hack; it is the object $(\mathbb R^n, \cdot)$, and it is the **unit of merging**: conditioning anything against it changes nothing. In canonical (information) form $\mathcal N^{-1}(\eta, \Lambda)$ this is $\Lambda = 0$.
- **Merging is adding information.** Conditioning two Gaussian beliefs about the same variable to agree multiplies densities — in canonical form, adds $(\eta, \Lambda)$. With $D \ne 0$ allowed, this operation is **total**, which is exactly what a Frobenius multiplication needs; in moment form (mean, covariance) it is not even expressible for singular precisions.
- **Constraints and noise in one language.** Ohm's law $V = RI$ as an exact linear relation, a noisy measurement as a Gaussian, "the mass is somewhere, no idea where" as a subspace — all are morphisms of one hypergraph category (Examples 1–2, 16). This is Willems' open stochastic systems made compositional.
- **A complete diagrammatic calculus.** *Graphical Quadratic Algebra* (Stein, Zanasi, Piedeleu & Samuelson) axiomatises quadratic relations by string-diagram equations and proves completeness, extending the [[Graphical Linear Algebra|interacting Hopf algebras]] of linear relations with a probabilistic layer.

## The cost

Normalisation is given up: a morphism of $\mathbf{GaussRel}$ may be improper, and the product of densities produced by a merge carries a normalising constant (the evidence) that the category does not track. An implementation must keep the log-partition function separately if it wants marginal likelihoods — the [[Partial Markov Category|partial]] perspective on the same bookkeeping.

## Lenticulum.jl

Lenticulum's `GaussianBelief` stores beliefs in canonical form with a precision that *may be singular*, and `combine` adds canonical parameters — which is precisely what makes its Gaussian fragment a hypergraph category. See [Acausal Composition is a Hypergraph Category](https://mathstruct.org/Lenticulum.jl/dev/vault/Factor-Graphs/Acausal-Composition-is-a-Hypergraph-Category).

````tabs
tab: Julia
**Docs:** [Theories (Catlab): ThHypergraphCategory](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/theories/)
```julia
using LinearAlgebra
# Gaussian beliefs in canonical form (η, Λ); Λ may be singular (improper). Merge = add.
struct Canon; η::Vector{Float64}; Λ::Matrix{Float64}; end
merge(a::Canon, b::Canon) = Canon(a.η + b.η, a.Λ + b.Λ)          # Frobenius multiplication μ
uninformative(n) = Canon(zeros(n), zeros(n, n))                    # its unit η
isproper(b::Canon) = rank(b.Λ) == size(b.Λ, 1)
mean(b::Canon) = b.Λ \ b.η
# a noisy measurement of x₁ + x₂ = 3 (σ² = 0.5): rank-one, improper on its own
a = [1.0 1.0]; meas = Canon(vec(a' * 3.0 / 0.5), a' * a / 0.5)
# an exact-ish constraint x₁ − x₂ = 1 (tiny noise) — also rank one
c = [1.0 -1.0]; law = Canon(vec(c' * 1.0 / 1e-8), c' * c / 1e-8)
isproper(meas), isproper(law)                                     # (false, false)
post = merge(merge(meas, law), uninformative(2))
isproper(post), round.(mean(post); digits = 4)                    # (true, [2.0, 1.0])
merge(meas, uninformative(2)).Λ == meas.Λ                          # the unit law: true
```
tab: Lean
```lean
import Mathlib
-- An extended Gaussian on ℝⁿ as data: a nondeterministic subspace D and a Gaussian on a complement.
structure ExtGaussian (n : ℕ) where
  D : Submodule ℝ (Fin n → ℝ)
  mean : Fin n → ℝ
  cov : Matrix (Fin n) (Fin n) ℝ      -- a covariance on a complement of D (positive semidefinite)
```
tab: Haskell
```haskell
-- 1-D Gaussian beliefs in canonical form: precision λ ≥ 0 (λ = 0 is the improper prior)
data Canon = Canon { eta :: Double, lam :: Double } deriving Show

merge :: Canon -> Canon -> Canon                 -- conditioning two beliefs to agree
merge (Canon e1 l1) (Canon e2 l2) = Canon (e1 + e2) (l1 + l2)

uninformative :: Canon
uninformative = Canon 0 0                         -- the unit of merge

meanOf :: Canon -> Maybe Double
meanOf (Canon e l) = if l > 0 then Just (e / l) else Nothing
-- meanOf (merge (Canon 2 1) (Canon 12 3)) == Just 3.5
```
````
