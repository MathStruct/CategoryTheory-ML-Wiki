#definition #example

In a [[Markov Category]], two parallel morphisms $f, g : X \to Y$ are **equal $p$-almost surely**, for a morphism (typically a state) $p : \Theta \to X$, written $f =_{p\text{-a.s.}} g$, if

$$
p \mathbin{;} \mathrm{copy}_X \mathbin{;} (1_X \otimes f) \;=\; p \mathbin{;} \mathrm{copy}_X \mathbin{;} (1_X \otimes g) \quad : \Theta \to X \otimes Y,
$$

i.e. the joint distributions of (input, output) agree when inputs are drawn from $p$. In $\mathbf{FinStoch}$ this means $f(y \mid x) = g(y \mid x)$ for every $x$ in the **support** of $p$; in $\mathbf{Stoch}$, that $f(\cdot \mid x)$ and $g(\cdot \mid x)$ agree for $p$-almost every $x$.

> Sources: Fritz [arXiv:1908.07021](https://arxiv.org/abs/1908.07021) ([[A Synthetic Approach to Markov Kernels, Conditional Independence and Theorems on Sufficient Statistics|notes]]) Definition 13.1, Examples 13.2–13.3, Lemmas 13.4–13.5, Definitions 13.11, 13.20 (supports); Cho & Jacobs [arXiv:1709.00322](https://arxiv.org/abs/1709.00322) ([[Disintegration and Bayesian Inversion via String Diagrams|notes]]) Definition 5.1, Propositions 5.2–5.4; Braithwaite, Hedges & St Clere Smithe [arXiv:2305.06112](https://arxiv.org/abs/2305.06112) ([[The Compositional Structure of Bayesian Inference|notes]]) Definition 6, Proposition 7; Braithwaite & Hedges [arXiv:2209.14728](https://arxiv.org/abs/2209.14728) ([[Dependent Bayesian Lenses|notes]]) Definitions 2, 4, 6.

## Why it is unavoidable

[[Conditionals and Disintegration|Conditionals]] and [[Bayesian Inversion|Bayesian inverses]] are **unique only almost surely** (Fritz Proposition 11.15; Braithwaite et al. Proposition 7): Bayes' law $p(x \mid y) = p(y \mid x) p(x) / p(y)$ says nothing about observations $y$ of probability zero, so any choice there is equally valid. Consequences:

- Bayesian inversion is a functor only **up to almost-sure equality** — the AutoBayes paper's footnote that $(-)^\dagger$ is "almost surely a pseudofunctor" ([[Lax Functor]]). Braithwaite & Hedges remove the ambiguity by passing to **supports**: inverses restricted to the support of the pushforward are unique (Theorem 20 of 2305.06112).
- Almost-sure equality is compatible with composition on the right (Lemma 13.4) and with pairing (Lemma 13.5), so one can reason modulo it.
- **Numerically**: an implementation that conditions on observations of (near-)zero pushforward density is exactly in the region where the inverse is undetermined; guards against zero-measure conditioning are the computational shadow of this definition.

## Example

Let $p = (1, 0)$ on $X = \{a, b\}$. Any two kernels $f, g : X \to Y$ with $f(\cdot \mid a) = g(\cdot \mid a)$ are $p$-a.s. equal, no matter what they do at $b$: $b$ is never observed. The Bayesian inverse of any $h : X \to Y$ at this prior is determined only on the outputs $h$ can actually produce from $a$.

````tabs
tab: Julia
**Docs:** [Theories (Catlab): copy/delete — ThMonoidalCategoryWithDiagonals](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/theories/)
```julia
# f =ₚ g  iff the joint laws (x, f(x)) and (x, g(x)) agree under x ~ p.
joint(p, f) = [p[x] * f[x, y] for x in eachindex(p), y in axes(f, 2)]
as_equal(p, f, g) = joint(p, f) ≈ joint(p, g)
p = [1.0, 0.0]
f = [0.2 0.8; 0.5 0.5]
g = [0.2 0.8; 0.9 0.1]              # differs only at the input b, which p never produces
as_equal(p, f, g), f ≈ g             # (true, false)
```
tab: Lean
```lean
import Mathlib
open MeasureTheory
-- In Mathlib, a.e.-equality of kernels w.r.t. a measure μ is `∀ᵐ x ∂μ, κ x = η x`.
example {α β : Type*} [MeasurableSpace α] [MeasurableSpace β] (μ : Measure α)
    (κ η : ProbabilityTheory.Kernel α β) : Prop :=
  ∀ᵐ x ∂μ, κ x = η x
```
tab: Haskell
```haskell
-- a.s. equality of finite kernels: compare only on the support of the prior
type Kernel x y = x -> [(y, Double)]

asEqual :: (Eq x, Eq y) => [(x, Double)] -> [y] -> Kernel x y -> Kernel x y -> Bool
asEqual prior ys f g = and [ abs (w y (f x) - w y (g x)) < 1e-12 | (x, p) <- prior, p > 0, y <- ys ]
  where w y d = sum [ q | (y', q) <- d, y' == y ]
```
````
