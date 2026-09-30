#definition #theorem #example

A **statistical game** $c : X \multimap Y$ (St Clere Smithe & Perin, *AutoBayes*, Definition 20) is a quadruple $(c, c', l^c, H^c)$ of

- a [[Bayesian Lens]] $(c, c') : X \mapsto Y$ — generative model and approximate inversion (over [[Open Model|open models]], so $c : X \rightsquigarrow [\![c]\!] \times Y$);
- an **energy** $l^c : X \times [\![c]\!] \times Y \to [0, \infty]$, evaluated at *points*;
- an **entropy** (regulariser) $H^c : \mathcal P X \times Y \to [0, \infty]$, evaluated at a *prior* and an observation;

which combine into a **loss** (generalised free energy)

$$
F^c(\pi, y) \;=\; \mathop{\mathbb E}_{(x, a) \sim c'_\pi(y)}\bigl[l^c(x, a, y)\bigr] \;-\; H^c(\pi, y).
$$

With $l^c = -\log p_c$ and $H^c$ the Shannon entropy of $c'_\pi(y)$ this is the [[Variational Free Energy]] (after composing with the prior, see below). "Game" is inherited from compositional game theory: the loss is a fitness function attached to a lens, as in [[Open Game|open games]], but with a single player.

> Sources: St Clere Smithe & Perin [arXiv:2503.18608](https://arxiv.org/abs/2503.18608) ([[AutoBayes - A Compositional Framework for Generalized Variational Inference|notes]]) Definitions 20, 22, 25, 27–29, Theorem 23, Remarks 21, 24, 26, 30, Appendix A (Examples 1–5); St Clere Smithe, *Compositional Active Inference I: Bayesian Lenses. Statistical Games* [arXiv:2109.04461](https://arxiv.org/abs/2109.04461) ([[Compositional Active Inference I - Bayesian Lenses and Statistical Games|notes]]) Proposition 4.11, Definitions 4.12, 5.8–5.11, Examples 5.3–5.17 (the earlier definition, via fitness functions on contexts, which AutoBayes supersedes); St Clere Smithe, thesis [arXiv:2212.12538](https://arxiv.org/abs/2212.12538) ([[Mathematical Foundations for a Compositional Account of the Bayesian Brain|notes]]).

## Composition: energies add, entropies chain (Definition 22)

For $c : X \multimap Y$ and $d : Y \multimap Z$, the composite $d \diamond c$ has the composite Bayesian lens and

$$
l^{dc}(x, a, y, b, z) = l^c(x, a, y) + l^d(y, b, z),
\qquad
H^{dc}(\pi, z) = \mathop{\mathbb E}_{(y,b) \sim d'_{c_*\pi}(z)}\bigl[H^c(\pi, y)\bigr] + H^d(c_*\pi, z).
$$

Energies are pointwise, so they simply add; entropies are functionals of distributions, so the upstream one is averaged under the downstream inversion and the downstream one is evaluated at the pushed-forward prior.

> **Theorem 23 (chain rule for free energy).**
> $$F^{dc}(\pi, z) \;=\; \mathop{\mathbb E}_{(y,b) \sim d'_{c_*\pi}(z)}\bigl[F^c(\pi, y)\bigr] \;+\; F^d(c_*\pi, z).$$

The additive energy and the chained entropy conspire to give one clean recursion for the total loss: **losses can be composed mechanically and locally, like gradients in differentiable programming**, instead of deriving an ELBO for every model by hand. Parallel composition adds both halves (Definition 25); its laxness, for Shannon entropies, is measured by the **mutual information** between the branches (Remark 26).

## Priors are games too (Remark 24)

A pure game $c$ with $l^c = -\log p_c(y \mid x)$ and Shannon entropy has loss $\mathbb E[-\log p_c(y \mid x)] - H(c'_\pi(y))$ — missing the $-\log p_\pi(x)$ term of the free energy. That is not an error: $c$ is *open*. Turn the prior $\pi : 1 \nrightarrow\!\!\!\bullet\ X$ into a game $\pi : 1 \multimap X$ with trivial inversion, energy $l^\pi = -\log p_\pi$ and zero entropy; then $F^{c\pi}(\ast, y) = \mathrm{VFE}(c, c')(\pi, y)$. **The prior is not part of the model; it is a separate game one composes with** — and so are data (cups) and losses. The identity game has zero energy and entropy, and games form a [[Bicategory]].

## Parameterized statistical games and their gradients

A **parameterized statistical game** (Definition 27) is a pair $(\Theta, c)$ of a space $\Theta$ and a function $c : \Theta \to \{X \multimap Y\}$ — any of $c, c', l, H$ may depend on $\theta$ (decoder, encoder, learned loss, $\beta$-annealing). This is $\mathbf{Para}$ of the category of games ([[Para Construction]]); composition multiplies parameter spaces (Definition 28). The intended semantics is descent of $F^c(\pi, y; \theta)$ with respect to the **Fisher metric** on $\Theta$ — the Bayesian learning rule of Khan & Rue, i.e. natural gradient.

Composing gradients locally (Definition 29) — stacking $\nabla_\varphi F^d$ with $\mathbb E[\nabla_\theta F^c]$ — keeps only the block-diagonal of the true Jacobian. It drops the terms where $\theta$ moves the pushforward prior and where $\varphi$ moves the sampling distribution of the inversion: exactly the reparametrisation / score-function terms of VAE training, i.e. "do you backprop through the sampler?". The gradient assignment is therefore **lax**, and formally a *lax section of a fibration* over parameterized games (Remark 30; [[Lax Functor]], [[Grothendieck Construction]]). Different approximation schemes (Laplace, delta rule, sampling) are different such sections — "different semantics functors".

## Worked examples (Appendix A)

| Example | wiring | what descending the loss is |
|---|---|---|
| 1 | Gaussian $c : M \multimap Y$ after a parameterized prior on mixing weights | maximum likelihood for a mixture |
| 2 | lens + prior, NLL energies, **zero entropies** | expectation–maximisation (E-step = evaluate, M-step = descend) |
| 3 | parameters moved into a wire, with a hyperprior | variational Bayesian EM |
| 4 | $X \otimes c$ after a **cup** on $X$ | supervised learning — the inversion trivialises |
| 5 | cup on $X$ only, prior on weights $\Theta$ | Bayesian deep learning |

## Relatives

| framework | backward pass | objective |
|---|---|---|
| [[Gradient-Based Learning with Parametric Lenses]] | gradient | loss via a learning-rate cap |
| statistical games | posterior | free energy, compositional |
| [[Open Game|open games]] | best response | utilities, Nash equilibrium |

## Lenticulum.jl

Lenticulum.jl factors are parameterized statistical games, with a **vector-valued** energy (a direct sum instead of the addition of Definition 22) so that Jacobians — and hence Gauss–Newton, Fisher metrics and the implicit function theorem — remain available; the gap between the two chain rules is a Jensen gap. See [Factors are Parameterized Statistical Games](https://mathstruct.org/Lenticulum.jl/dev/vault/Foundations/Factors-are-Parameterized-Statistical-Games) and [Scalar and Multivariate Energy](https://mathstruct.org/Lenticulum.jl/dev/vault/Foundations/Scalar-and-Multivariate-Energy).

````tabs
tab: Julia
**Docs:** [Theories (Catlab): copy/delete — ThMonoidalCategoryWithDiagonals](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/theories/)
```julia
# Theorem 23 on a finite two-stage model: F^{dc} = E_{y∼d'}[F^c(π, y)] + F^d(c∗π, z).
π = [0.4, 0.6]                        # prior on X
c = [0.7 0.3; 0.2 0.8]                # X → Y
d = [0.9 0.1; 0.3 0.7]                # Y → Z
bayes(p, k, y) = (w = [p[x] * k[x, y] for x in eachindex(p)]; w ./ sum(w))
H(q) = -sum(qi * log(qi) for qi in q if qi > 0)
# games with exact inversions, NLL energies and Shannon entropies
Fc(p, y) = (q = bayes(p, c, y); sum(q[x] * -log(c[x, y]) for x in 1:2) - H(q))
Fd(p, z) = (q = bayes(p, d, z); sum(q[y] * -log(d[y, z]) for y in 1:2) - H(q))
z = 2
pushed = vec(π' * c)
dinv = bayes(pushed, d, z)            # d′_{c∗π}(z)
lhs_chain = sum(dinv[y] * Fc(π, y) for y in 1:2) + Fd(pushed, z)
# direct computation of the composite game's loss from the composite energy and entropy
joint = [π[x] * c[x, y] * d[y, z] for x in 1:2, y in 1:2]; post = joint ./ sum(joint)
energy_dc = sum(post[x, y] * (-log(c[x, y]) - log(d[y, z])) for x in 1:2, y in 1:2)
entropy_dc = sum(dinv[y] * H(bayes(π, c, y)) for y in 1:2) + H(dinv)
lhs_chain ≈ energy_dc - entropy_dc   # true: energies add, entropies chain
# adding the prior game (energy −log π, zero entropy) gives the surprisal −log p(z)
lhs_chain + sum(post[x, y] * -log(π[x]) for x in 1:2, y in 1:2) ≈ -log(sum(joint))   # true
```
tab: Lean
```lean
import Mathlib
-- A statistical game over finite types, as data (AutoBayes Definition 20, simplified: no latents).
structure StatGame (X Y : Type) where
  fwd : X → Y → ℝ                    -- likelihood c(y | x)
  inv : (X → ℝ) → Y → X → ℝ          -- prior ↦ approximate posterior c'_π(· | y)
  energy : X → Y → ℝ                 -- l^c, pointwise
  entropy : (X → ℝ) → Y → ℝ          -- H^c, a functional of the prior

-- the loss F^c(π, y) = E_{x ∼ c'_π(y)} [l^c(x, y)] − H^c(π, y)
noncomputable def StatGame.loss {X Y : Type} [Fintype X] (g : StatGame X Y) (π : X → ℝ) (y : Y) : ℝ :=
  (∑ x, g.inv π y x * g.energy x y) - g.entropy π y
```
tab: Haskell
```haskell
-- The free-energy chain rule, checked numerically on a finite model
bayes :: [Double] -> [[Double]] -> Int -> [Double]
bayes p k y = let w = [ px * (row !! y) | (px, row) <- zip p k ] in map (/ sum w) w

entropy :: [Double] -> Double
entropy q = negate (sum [ x * log x | x <- q, x > 0 ])

loss :: [[Double]] -> [Double] -> Int -> Double         -- exact inversion, NLL energy, Shannon entropy
loss k p y = let q = bayes p k y in sum [ qx * negate (log (row !! y)) | (qx, row) <- zip q k ] - entropy q

push :: [Double] -> [[Double]] -> [Double]
push p k = [ sum [ px * (row !! j) | (px, row) <- zip p k ] | j <- [0 .. length (head k) - 1] ]

chain :: [Double] -> [[Double]] -> [[Double]] -> Int -> Double   -- E_{y∼d'}[F^c] + F^d
chain p c d z = let dinv = bayes (push p c) d z
                in sum [ w * loss c p y | (y, w) <- zip [0 ..] dinv ] + loss d (push p c) z
```
````
