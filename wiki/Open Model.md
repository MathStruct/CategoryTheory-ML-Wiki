#definition #example

An **open model** $p : X \nrightarrow\!\!\!\bullet\ Y$ (St Clere Smithe & Perin, *AutoBayes*, Definition 1) is a measurable space $[\![p]\!]$ — the **latent space** — together with a Markov kernel

$$
p : X \rightsquigarrow [\![p]\!] \times Y .
$$

$X$ is the **unobserved** space (what inference is about), $Y$ the **observed** space (where data lives), and $[\![p]\!]$ holds intermediate quantities that became hidden when models were composed. Special cases: a **pure model** has $[\![p]\!] \cong 1$ (an ordinary kernel $X \rightsquigarrow Y$); a **pure distribution** has $X \cong [\![p]\!] \cong 1$ (a prior); a **joint distribution** has $X \cong 1$ but non-trivial latent space. "Open" means open to composition — a conditional distribution; only $1 \nrightarrow\!\!\!\bullet\ 1$ is closed.

> Sources: St Clere Smithe & Perin [arXiv:2503.18608](https://arxiv.org/abs/2503.18608) ([[AutoBayes - A Compositional Framework for Generalized Variational Inference|notes]]) Definitions 1, 4, 6, Remarks 2, 3, 5, 7, 8 (and the discussion of Bayesian networks citing Fong, [arXiv:1301.6201](https://arxiv.org/abs/1301.6201) ([[Causal Theories - A Categorical Perspective on Bayesian Networks|notes]]), Theorem 4.5); St Clere Smithe, thesis [arXiv:2212.12538](https://arxiv.org/abs/2212.12538) ([[Mathematical Foundations for a Compositional Account of the Bayesian Brain|notes]]) §5.2.1 (copy-composition by coparameterization, $\mathbf{Copara}_2$); Capucci et al. [arXiv:2105.06332](https://arxiv.org/abs/2105.06332) ([[Towards Foundations of Categorical Cybernetics|notes]]) Remark 4 ($\mathbf{CoPara}$).

## Composition without integration (Definition 4)

For $p : X \nrightarrow\!\!\!\bullet\ Y$ and $q : Y \nrightarrow\!\!\!\bullet\ Z$,

$$
[\![q \circ p]\!] = [\![p]\!] \times Y \times [\![q]\!], \qquad (q \circ p)(ds, dy, dt, dz \mid x) = q(dt, dz \mid y)\; p(ds, dy \mid x).
$$

Compare the ordinary Chapman–Kolmogorov composite $\int_y q(dz \mid y)\, p(dy \mid x)$: open models **never integrate** — the intermediate value $y$ is *filed away* in the latent space instead of marginalised. Composites of pure models are not pure. The price is memory (the latent space of a deep composite is the product of all intermediate spaces — the analogue of an activation cache); the gain is that the [[Bayesian Inversion]] chain rule holds without computing marginals (Theorem 13). Structurally this is **copy-composition by coparameterization**: open models are coparametrised kernels ([[Para Construction|CoPara]]), with the latent space as coparameter.

Composition is associative up to reassociating latent products, so open models form a [[Bicategory]] (Remark 5) with the Dirac kernel as identity; parallel composition $[\![q \otimes q']\!] = [\![q]\!] \times [\![q']\!]$ makes it monoidal (Definition 6, Remark 7).

## Derived operations

- **Reveal** — a 2-cell moving a latent factor into the observed space, $\mathrm{reveal}_A(q \circ p) : 1 \nrightarrow\!\!\!\bullet\ A \otimes B$; a retyping, not a computation (exposing an intermediate activation as an output).
- **Dummy variables** — $A \otimes q := \mathrm{id}_A \otimes q$, letting information flow past a model untouched (a skip connection; "this factor does not depend on that variable").
- **Every Bayesian network is a composite** of open models: topologically sort, reveal each node's parents, pad with dummies, compose in order (after Fong 2013, Theorem 4.5).

## Copiers, cups and caps (Remark 8)

$$
\mathrm{copy}(da_1, da_2 \mid a_0) = [a_1 = a_0 = a_2]\, da_1\, da_2, \qquad
\mathrm{cup}_A = [a_1 = a_2]\, da_1\, da_2 : 1 \nrightarrow\!\!\!\bullet\ A \otimes A, \qquad
\mathrm{cap}_A(a_1, a_2) = [a_1 = a_2] .
$$

The cap is an *unnormalised* kernel $A \times A \rightsquigarrow 1$. With cups and caps the bicategory of open models is **self-dual [[Compact Closed Category|compact closed]]**: a cup turns an unobserved leg into an observed one, which is how supervised learning ("clamp the labels", AutoBayes Example 4) and cyclic models (feedback, mutually recursive latents) are written. Admitting unnormalised measures relaxes the DAG restriction of Bayesian networks, at the price of a partition function — the same trade as [[Partial Markov Category|conditioning]] and as moving from a [[Markov Category]] to a [[Hypergraph Category]].

## Lenticulum.jl

`AbstractOpenModel`, `forward` (returning the latent part separately), `pushforward`, `latentspace` and the `Latent()` channel polarity implement open models. See [Open Models and Latent Channels](https://mathstruct.org/Lenticulum.jl/dev/vault/Foundations/Open-Models-and-Latent-Channels).

````tabs
tab: Julia
**Docs:** [Theories (Catlab): copy/delete — ThMonoidalCategoryWithDiagonals](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/theories/)
```julia
# Finite open models: X → ⟦p⟧ × Y as a Dict from x to a distribution over (latent, y) pairs.
openmodel(f) = f                                   # x ↦ Vector{Pair{Tuple{latent,y},Float64}}
function compose(q, p)                              # no integration: y is filed into the latent
    x -> [((s, y, t), z) => w1 * w2 for (((s, y), w1)) in p(x) for (((t, z), w2)) in q(y)]
end
cloud_to_humidity(x) = x == :cloudy ? [((nothing, :humid), 0.8), ((nothing, :dry), 0.2)] :
                                       [((nothing, :humid), 0.3), ((nothing, :dry), 0.7)]
humidity_to_rain(h)  = h == :humid  ? [((nothing, :rain), 0.6), ((nothing, :none), 0.4)] :
                                       [((nothing, :rain), 0.1), ((nothing, :none), 0.9)]
rain = compose(humidity_to_rain, cloud_to_humidity)
out = rain(:cloudy)
length(out)                                         # 4: humidity survives, as latent
sum(w for (_, w) in out) ≈ 1                        # still a probability kernel
# marginalising the latent (the expensive step open models defer) gives the pure composite:
p_rain = sum(w for ((lat, z), w) in out if z == :rain)
p_rain ≈ 0.8 * 0.6 + 0.2 * 0.1                      # true
```
tab: Lean
```lean
import Mathlib
open MeasureTheory ProbabilityTheory
-- An open model X ⇸ Y with latent space L: a Markov kernel X → L × Y.
structure OpenModel (X L Y : Type) [MeasurableSpace X] [MeasurableSpace L] [MeasurableSpace Y] where
  kernel : Kernel X (L × Y)
  isMarkov : IsMarkovKernel kernel
```
tab: Haskell
```haskell
-- Open models over finite distributions: x ↦ distribution over (latent, y)
newtype Dist a = Dist { runDist :: [(a, Double)] }
newtype Open l x y = Open { run :: x -> Dist (l, y) }

-- composition files the intermediate y into the latent space: no summation happens
compose :: Open m y z -> Open l x y -> Open (l, y, m) x z
compose (Open q) (Open p) = Open $ \x -> Dist
  [ (((s, y, t), z), w1 * w2) | ((s, y), w1) <- runDist (p x), ((t, z), w2) <- runDist (q y) ]
```
````
