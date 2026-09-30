#definition #theorem #example

For a [[Bayesian Lens]] $(c, c')$ — a model $c$ with likelihood $p_c(y \mid x)$, a prior $\pi$ with density $p_\pi$, and an approximate inversion $c'_\pi(y)$ — the **variational free energy** at an observation $y$ is

$$
\mathrm{VFE}(c, c')(\pi, y) \;=\; D_{\mathrm{KL}}\bigl(c'_\pi(y) \,\big\|\, c^\dagger_\pi(y)\bigr) \;-\; \log p_{c_* \pi}(y),
$$

the divergence of the approximate posterior from the exact one ([[Bayesian Inversion]]) plus the **surprisal** (negative log-evidence) of the observation. Since $D_{\mathrm{KL}} \ge 0$ it is an upper bound on $-\log p(y)$ — the *evidence upper bound*, i.e. the negative ELBO. The miracle is that it is computable although both terms separately are not:

$$
\mathrm{VFE}(c, c')(\pi, y) \;=\; \underbrace{\mathop{\mathbb E}_{x \sim c'_\pi(y)}\bigl[-\log p_c(y \mid x) - \log p_\pi(x)\bigr]}_{\text{expected energy}} \;-\; \underbrace{H\bigl(c'_\pi(y)\bigr)}_{\text{entropy}} .
$$

The intractable posterior $c^\dagger_\pi$ **cancels** against the evidence term.

> Sources: St Clere Smithe & Perin, *AutoBayes* [arXiv:2503.18608](https://arxiv.org/abs/2503.18608) ([[AutoBayes - A Compositional Framework for Generalized Variational Inference|notes]]) §4 (the KL chain rule), Definition 17, Proposition 18, Remark 19; St Clere Smithe [arXiv:2109.04461](https://arxiv.org/abs/2109.04461) ([[Compositional Active Inference I - Bayesian Lenses and Statistical Games|notes]]) Definitions 5.8–5.11, Proposition 5.10; Knoblauch, Jewson & Damoulas (2019), *Generalized variational inference*; Khan & Rue (2021), *The Bayesian learning rule*; LeCun et al. (2006), *A tutorial on energy-based learning* (the physics terminology).

## Three forms (AutoBayes Proposition 18)

$$
\begin{aligned}
\mathrm{VFE} &= \mathop{\mathbb E}_{(x,a) \sim c'_\pi(y)}\bigl[\log p_{c'_\pi}(x, a \mid y) - \log p_{c^\dagger_\pi}(x, a \mid y)\bigr] - \log p_{c_Y \bullet \pi}(y) \\
&= \mathop{\mathbb E}_{(x,a) \sim c'_\pi(y)}\bigl[\log p_{c'_\pi}(x, a \mid y) - \log p_c(a, y \mid x) - \log p_\pi(x)\bigr] \\
&= \mathop{\mathbb E}_{(x,a) \sim c'_\pi(y)}\bigl[-\log p_c(a, y \mid x) - \log p_\pi(x)\bigr] - H\bigl(c'_\pi(y)\bigr)
\end{aligned}
$$

(with $a$ ranging over the latent space of an [[Open Model|open model]]). The second line is where $c^\dagger$ disappears; the third is the Helmholtz form *energy − entropy* of statistical physics.

## The compositional observation

The KL divergence itself has a chain rule — for $(d, d') \circ (c, c')$,
$\mathrm{KL}[\ldots](\pi, z) = \mathbb E_{(y,b) \sim d'_{c_*\pi}(z)}\bigl[\mathrm{KL}(c, c')(\pi, y)\bigr] + \mathrm{KL}(d, d')(c_*\pi, z)$ — but it is useless, since it needs $c^\dagger$. The free energy is computable, and in its third form the two halves **compose differently**:

> Energies compose by simple addition, but entropies (and thus losses built from them) compose like the chain rule. (AutoBayes, §4)

So one should not carry the free energy as a single number but as the pair (energy, entropy) — which is exactly a [[Statistical Game]]. Remark 19 notes that generalized VI (Knoblauch et al.) and the Bayesian learning rule (Khan & Rue) split objectives the same way but do not exploit the compositional consequence.

## Special cases

| choice | free energy becomes |
|---|---|
| exact inversion $c' = c^\dagger$ | $-\log p(y)$: the negative log-evidence (KL term vanishes) |
| zero entropy, point-mass $c'$ | $-\log p(y, \hat x)$: MAP / maximum likelihood |
| encoder $q_\phi$, decoder $p_\theta$ | the negative ELBO of a VAE |
| a temperature $T$ on the entropy, $T \to 0$ | the energy $\min_x E(x, y)$ of an energy-based model |

````tabs
tab: Julia
**Docs:** [Theories (Catlab): copy/delete — ThMonoidalCategoryWithDiagonals](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/theories/)
```julia
# VFE on a finite model: energy − entropy equals KL(q‖posterior) − log evidence.
π = [0.3, 0.7]; c = [0.8 0.2; 0.1 0.9]        # prior on X, likelihood X → Y
y = 1
evidence = sum(π[x] * c[x, y] for x in 1:2)
posterior = [π[x] * c[x, y] / evidence for x in 1:2]
q = [0.6, 0.4]                                  # an approximate posterior c′_π(y)
KL(q, p) = sum(q[i] * log(q[i] / p[i]) for i in eachindex(q) if q[i] > 0)
energy  = sum(q[x] * (-log(c[x, y]) - log(π[x])) for x in 1:2)
entropy = -sum(q[x] * log(q[x]) for x in 1:2)
vfe = energy - entropy
vfe ≈ KL(q, posterior) - log(evidence)          # Proposition 18: true
vfe ≥ -log(evidence)                             # an upper bound on surprisal: true
energy - entropy ≈ -log(evidence) + KL(q, posterior)   # equality exactly when q = posterior
```
tab: Lean
```lean
import Mathlib
open MeasureTheory
-- Gibbs' inequality (KL ≥ 0), the reason the VFE bounds the surprisal, rests on log x ≤ x − 1:
#check @Real.log_le_sub_one_of_pos
-- recent Mathlib also has the KL divergence of measures: `InformationTheory.klDiv`
```
tab: Haskell
```haskell
-- variational free energy on a finite space
vfe :: [Double] -> [Double] -> [Double] -> Double   -- prior, likelihood column at y, q
vfe prior like q = energy - entropy
  where energy  = sum [ qi * (negate (log li) - log pi') | (qi, li, pi') <- zip3 q like prior, qi > 0 ]
        entropy = negate (sum [ qi * log qi | qi <- q, qi > 0 ])

surprisal :: [Double] -> [Double] -> Double
surprisal prior like = negate (log (sum (zipWith (*) prior like)))
-- vfe [0.3,0.7] [0.8,0.1] [0.6,0.4] >= surprisal [0.3,0.7] [0.8,0.1]
```
````
