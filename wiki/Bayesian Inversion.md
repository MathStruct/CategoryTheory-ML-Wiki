#definition #theorem #example

In a [[Markov Category]], let $p : I \to X$ be a state (a **prior**) and $f : X \to Y$ a kernel (a **likelihood**, or channel). A **Bayesian inverse** of $f$ at $p$ is a kernel $f^\dagger_p : Y \to X$ such that the joint distribution can be produced in either direction:

$$
p \mathbin{;} \mathrm{copy}_X \mathbin{;} (1_X \otimes f) \;=\; (f_* p) \mathbin{;} \mathrm{copy}_Y \mathbin{;} (f^\dagger_p \otimes 1_Y) \quad : I \to X \otimes Y,
$$

"sample $x \sim p$, then $y \sim f(\cdot \mid x)$" equals "sample $y$ from the pushforward $f_* p$, then $x \sim f^\dagger_p(\cdot \mid y)$". In $\mathbf{FinStoch}$ this is **Bayes' law**,

$$
f^\dagger_p(x \mid y) \;=\; \frac{f(y \mid x)\, p(x)}{\sum_{x'} f(y \mid x')\, p(x')} \qquad \text{wherever } (f_*p)(y) > 0 .
$$

Bayesian inversion is a special [[Conditionals and Disintegration|disintegration]]: integrate $p$ and $f$ into a joint state, then disintegrate it the other way (Cho & Jacobs §3).

> Sources: Cho & Jacobs [arXiv:1709.00322](https://arxiv.org/abs/1709.00322) ([[Disintegration and Bayesian Inversion via String Diagrams|notes]]) §3 (Eq. (5), Examples 3.8–3.9, Proposition 3.10), §7; Fritz [arXiv:1908.07021](https://arxiv.org/abs/1908.07021) ([[A Synthetic Approach to Markov Kernels, Conditional Independence and Theorems on Sufficient Statistics|notes]]) Proposition 11.17, Remark 13.10; Braithwaite, Hedges & St Clere Smithe [arXiv:2305.06112](https://arxiv.org/abs/2305.06112) ([[The Compositional Structure of Bayesian Inference|notes]]) Definition 1, Proposition 11, Theorem 20; St Clere Smithe [arXiv:2006.01631](https://arxiv.org/abs/2006.01631) ([[Bayesian Updates Compose Optically|notes]]) §2, Theorem 5.2; St Clere Smithe & Perin [arXiv:2503.18608](https://arxiv.org/abs/2503.18608) ([[AutoBayes - A Compositional Framework for Generalized Variational Inference|notes]]) §3, Theorem 13; Clerc, Danos, Dahlqvist & Garnier (2017), *Pointless learning*.

## The chain rule

Inverting a composite is composing the inverses **in reverse order**, with the *pushed-forward* prior for the second one:

$$
(f \mathbin{;} g)^\dagger_p \;=\; g^\dagger_{f_* p} \mathbin{;} f^\dagger_p \qquad (\text{almost surely}).
$$

Compare the reverse-mode chain rule $\mathrm d_x(g \circ f) = \mathrm d_x f \circ \mathrm d_{f(x)} g$:

| autodiff | Bayes |
|---|---|
| point $x$ | prior $p$ |
| pushforward point $f(x)$ | pushforward prior $f_* p$ |
| $\mathrm d_x f$ (transpose) | $f^\dagger_p$ |
| composite reverses order | composite reverses order |

**The prior plays the role of the linearisation point.** The chain rule makes Bayesian inversion a functor into [[Bayesian Lens|Bayesian lenses]] — exactly, or up to [[Almost-Sure Equality|almost-sure equality]] (Braithwaite et al. Proposition 11; St Clere Smithe Theorem 5.2), and on the nose once inverses are restricted to supports (Braithwaite et al. Theorem 20). AutoBayes (Theorem 13) proves the analogue for open models, where it holds without integrating out intermediate variables.

## Inversion as a dagger

On the category $\mathbf{ProbStoch}(\mathcal C)$ of *probability spaces* $(X, p)$ and measure-preserving kernels (modulo a.s. equality), Bayesian inversion is a **symmetric monoidal [[Dagger Category|dagger]]** (Fritz Remark 13.10; Clerc et al.): $(g f)^\dagger = f^\dagger g^\dagger$, $f^{\dagger\dagger} = f$. The dagger is "run the channel backwards, given where you started".

## Why it is the hard part of inference

Computing $f^\dagger_p$ requires the normaliser $(f_*p)(y) = \int f(y \mid x)\,p(dx)$ — a marginalisation, generally intractable. Every approximate-inference method replaces the exact inverse by a *chosen* kernel of the same type — amortised encoder, mean-field Gaussian, particle set, solver fixed point — and measures the error with a divergence; the categorical bookkeeping for that is the [[Bayesian Lens]] and the [[Statistical Game]].

## Lenticulum.jl

A Lenticulum factor-to-variable message is a Bayesian inversion $c'_\pi$ of the factor at the belief of the other variables. See [Messages are Inversions](https://mathstruct.org/Lenticulum.jl/dev/vault/Factor-Graphs/Messages-are-Inversions).

````tabs
tab: Julia
**Docs:** [Theories (Catlab): copy/delete — ThMonoidalCategoryWithDiagonals](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/theories/)
```julia
# Bayes' law in FinStoch and the chain rule (f;g)†_p = g†_{f*p} ; f†_p.
pushforward(p, f) = vec(p' * f)
function invert(p, f)                                   # f†_p : Y → X, rows = y
    q = pushforward(p, f)
    [q[y] > 0 ? f[x, y] * p[x] / q[y] : 1 / length(p) for y in axes(f, 2), x in eachindex(p)]
end
p = [0.2, 0.5, 0.3]                                     # prior on X
f = [0.9 0.1; 0.4 0.6; 0.1 0.9]                         # X → Y
g = [0.7 0.2 0.1; 0.1 0.3 0.6]                          # Y → Z
lhs = invert(p, f * g)                                   # (f;g)†_p : Z → X
rhs = invert(pushforward(p, f), g) * invert(p, f)        # g†_{f*p} ; f†_p
lhs ≈ rhs                                                # true
# the defining equation: both ways of sampling give the same joint on X × Y
q = pushforward(p, f); fd = invert(p, f)
[p[x] * f[x, y] for x in 1:3, y in 1:2] ≈ [q[y] * fd[y, x] for x in 1:3, y in 1:2]   # true
```
tab: Lean
```lean
import Mathlib
open MeasureTheory ProbabilityTheory
-- Mathlib: the posterior kernel is the Bayesian inverse of κ at the prior μ,
-- characterised by μ ⊗ₘ κ = (κ ∘ₘ μ) ⊗ₘ κ†μ up to swapping the factors.
#check @posterior
-- (see the lemmas in Mathlib.Probability.Kernel.Posterior)
```
tab: Haskell
```haskell
-- Bayes' law for finite channels
type Channel x y = x -> [(y, Double)]

pushforward :: Eq y => [(x, Double)] -> Channel x y -> [y] -> [(y, Double)]
pushforward p f ys = [ (y, sum [ px * w | (x, px) <- p, (y', w) <- f x, y' == y ]) | y <- ys ]

invert :: Eq y => [(x, Double)] -> Channel x y -> [y] -> Channel y x
invert p f ys y = [ (x, px * like x / py) | (x, px) <- p, py > 0 ]
  where like x = sum [ w | (y', w) <- f x, y' == y ]
        py = sum [ px * like x | (x, px) <- p ]
```
````
