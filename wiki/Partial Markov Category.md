#definition #theorem #example

A **partial Markov category** (Di Lavore, Román & Sobociński, Definition 3.1) is a [[Copy-Discard Category]] **with [[Conditionals and Disintegration|conditionals]]** (Definition 2.6). Unlike in a [[Markov Category]], morphisms need not be **total** ($f \mathbin{;} \mathrm{del} = \mathrm{del}$, Definition 2.1): a non-total morphism may **fail**, losing probability mass, and conditionals are only *almost surely* total (Proposition 3.2). The total morphisms form a Markov category. The prototype is the Kleisli category of the **Maybe monad** over a Markov category (Theorem 3.25): a morphism $X \to Y$ is a sub-stochastic kernel $X \to \mathcal D(Y + 1)$ whose missing mass is the probability of failure.

Partiality is what makes **conditioning** expressible inside the category: an *observation* is a partial map that fails when the observed event does not happen, and **normalisation** $n(f)$ (Definition 3.8) rescales the surviving mass. Bayes' theorem becomes an equation between string diagrams.

> Sources: Di Lavore, Román & Sobociński, *Partial Markov Categories* [arXiv:2502.03477](https://arxiv.org/abs/2502.03477) ([[Partial Markov Categories|notes]]) Definitions 2.1, 2.3, 2.6, 3.1, 3.3, 3.8, 3.14, Propositions 3.2, 3.11–3.12, Theorems 3.25, 4.6, 5.4, Definitions 4.7–4.8, 5.1; Cho & Jacobs [arXiv:1709.00322](https://arxiv.org/abs/1709.00322) ([[Disintegration and Bayesian Inversion via String Diagrams|notes]]) §7 (conditioning via partial channels); Stein & Staton (2021), *Compositional semantics for probabilistic programs with exact conditioning*.

## Examples

| partial Markov category | morphisms | total part |
|---|---|---|
| $\mathrm{Kl}(\mathcal D(- + 1))$ on sets | sub-stochastic channels $X \to \mathcal D(Y + 1)$ | $\mathbf{Stoch}$ (finite supports) |
| sub-stochastic kernels on standard Borel spaces (Def. 3.28) | subprobability kernels | $\mathbf{BorelStoch}$ |
| $\mathrm{Kl}(\mathrm{Maybe})$ over any Markov category (Thm 3.25) | kernels that may fail | the original Markov category |
| relations (cartesian bicategories of relations) | partial relations | total relations |

## What it buys

- **Exact observations.** An observation of $y_0$ is the partial map "succeed iff $y = y_0$". Composing a generative model with it and normalising is Bayesian conditioning; Section 5 builds a category of exact observations in which a synthetic Bayes' theorem holds (Theorem 5.4).
- **Two update rules compared.** Pearl's and Jeffrey's rules for updating on soft evidence (Definitions 4.7–4.8) are formalised and compared (Proposition 4.9).
- **Normalisation is compositional.** $n(n(f)) = n(f)$ and $n(f \mathbin{;} g) = n(n(f) \mathbin{;} g)$ (Propositions 3.11–3.12): one may normalise early or late.
- **The missing mass is evidence.** The failure probability of "run the model, then observe" is $1 - p(y_0)$; keeping it separately — rather than normalising it away — is keeping the marginal likelihood, which is what model comparison and free-energy objectives use.

## Where it sits

A Markov category cannot merge wires (normalisation forbids it); a [[Hypergraph Category]] can, by allowing unnormalised morphisms outright. Partial Markov categories are the middle ground: copying is free, deletion is natural only on the total part, and conditioning is available as a partial operation. Unnormalised message passing in factor graphs — carrying a normaliser alongside each message — is the computational version of the same bookkeeping.

````tabs
tab: Julia
**Docs:** [Theories (Catlab): copy/delete — ThMonoidalCategoryWithDiagonals](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/theories/)
```julia
# Sub-stochastic channels = stochastic channels into Y + {fail}. Observation and normalisation.
prior = Dict(:ill => 0.01, :well => 0.99)
test(x) = x == :ill ? Dict(:pos => 0.95, :neg => 0.05) : Dict(:pos => 0.10, :neg => 0.90)
observe(y0) = y -> y == y0 ? Dict(y => 1.0) : Dict{Symbol,Float64}()   # partial: fails otherwise
# run: x ~ prior, y ~ test(x), observe y = :pos, keep x
joint = Dict(x => px * sum(py * sum(values(observe(:pos)(y)); init = 0.0)
                           for (y, py) in test(x)) for (x, px) in prior)
evidence = sum(values(joint))                      # total surviving mass = p(pos) = 0.1085
normalise(d) = Dict(k => v / sum(values(d)) for (k, v) in d)
posterior = normalise(joint)
round(posterior[:ill]; digits = 4)                 # 0.0876: Bayes' theorem
1 - evidence                                       # failure mass = 1 − p(pos)
```
tab: Lean
```lean
import Mathlib
-- Sub-stochastic kernels: probability measures on `Option β` (none = failure).
open MeasureTheory ProbabilityTheory
example (α β : Type) [MeasurableSpace α] [MeasurableSpace β] : Type :=
  Kernel α (Option β)
#check @PMF.filter            -- conditioning a PMF on a set by restricting and normalising
#check @PMF.normalize
```
tab: Haskell
```haskell
-- Partial channels as Maybe-valued distributions; normalisation divides by the surviving mass.
type Dist a = [(a, Double)]

observe :: Eq y => y -> y -> Maybe y
observe y0 y = if y == y0 then Just y else Nothing

condition :: Dist x -> (x -> Dist y) -> (y -> Maybe y) -> (Dist x, Double)
condition prior k obs = (map (\(x, w) -> (x, w / ev)) surviving, ev)
  where surviving = [ (x, px * py) | (x, px) <- prior, (y, py) <- k x, Just _ <- [obs y] ]
        ev = sum (map snd surviving)
```
````
