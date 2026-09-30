#definition #theorem #example

A **Markov category** (Fritz, Definition 2.1) is a [[Symmetric Monoidal Category]] in which every object $X$ carries a commutative comonoid $\mathrm{copy}_X : X \to X \otimes X$, $\mathrm{del}_X : X \to I$, compatible with $\otimes$, such that **deletion is natural**: $f \mathbin{;} \mathrm{del}_Y = \mathrm{del}_X$ for every $f : X \to Y$. Equivalently, it is an *affine* [[Copy-Discard Category]]: the monoidal unit $I$ is terminal.

Think of a morphism $f : X \to Y$ as a **Markov kernel** — an assignment of a probability distribution on $Y$ to each input — composed by the Chapman–Kolmogorov formula. $\mathrm{copy}$ duplicates a value, $\mathrm{del}$ throws it away, and naturality of $\mathrm{del}$ says every kernel is **normalised**: running it and discarding the result is the same as not running it. A morphism $p : I \to X$ is a **state** (a distribution on $X$); $f \circ p$ is the **pushforward** $f_* p$; marginalisation is post-composition with $1 \otimes \mathrm{del}$.

> Sources: Fritz, *A synthetic approach to Markov kernels, conditional independence and theorems on sufficient statistics* [arXiv:1908.07021](https://arxiv.org/abs/1908.07021) ([[A Synthetic Approach to Markov Kernels, Conditional Independence and Theorems on Sufficient Statistics|notes]]) Definition 2.1, Examples 2.5–2.6, §3–4, Proposition 6.1, Definitions 10.1, 10.14, Remark 10.13, Definitions 11.1, 11.5, 11.31, 12.12, 12.16, 13.1; Cho & Jacobs [arXiv:1709.00322](https://arxiv.org/abs/1709.00322) ([[Disintegration and Bayesian Inversion via String Diagrams|notes]]) (affine CD categories); Golubtsov (2002), *Information transformers*; Fritz, Gonda, Perrone & Fjeldgren Rischel [arXiv:2010.07416](https://arxiv.org/abs/2010.07416) ([[Representable Markov Categories and Comparison of Statistical Experiments|notes]]); Perrone, *Markov Categories and Entropy* [arXiv:2212.11719](https://arxiv.org/abs/2212.11719) ([[Markov Categories and Entropy|notes]]).

## Examples

| Markov category | objects | morphisms $X \to Y$ | source |
|---|---|---|---|
| $\mathbf{FinStoch}$ | finite sets | stochastic matrices $f(y \mid x)$ | Fritz Ex. 2.5 |
| $\mathbf{Stoch}$ | measurable spaces | measurable Markov kernels $= \mathrm{Kl}(\mathcal G)$ | Fritz §4; [[Giry Monad]] |
| $\mathbf{BorelStoch}$ | standard Borel spaces | Markov kernels | Fritz Ex. 10.5 |
| $\mathbf{Gauss}$ | $\mathbb R^n$ | $x \mapsto \mathcal N(Mx + s, C)$ | Fritz §6 |
| $\mathrm{Kl}(T)$ | sets | $X \to TY$ for a commutative **affine** monad $T$ | Fritz Proposition 3.1, Corollary 3.2 |
| $\mathbf{FinSetMulti}$ | finite sets | non-empty multivalued maps (possibilistic) | Fritz Ex. 2.6 |
| any cartesian category | — | ordinary maps (deterministic "probability") | Fritz Remark 2.4 |

The [[Distribution Monad]] (finitely supported probability) and the non-empty [[Power Set Monad|power set]] monad give $\mathrm{Kl}(\mathcal D)$ and $\mathrm{Kl}(\mathcal P^+)$; both are affine because the only distribution on a one-point set is the Dirac.

## Structure inside a Markov category

- **Deterministic morphisms** (Definition 10.1): $f$ commutes with copying. They form a *cartesian* subcategory $\mathcal C_{\mathrm{det}}$ (Remark 10.13) — the "functions" inside the probabilistic world. In $\mathbf{FinStoch}$ they are the 0/1 matrices, in $\mathbf{Stoch}$ the kernels $\delta_{g(x)}$ for (essentially) measurable functions $g$.
- **Conditionals** (Definitions 11.1, 11.5): $f : A \to X \otimes Y$ factors through its $X$-marginal and a kernel $f_{|X} : X \otimes A \to Y$. Having conditionals is an axiom — true in $\mathbf{FinStoch}$, $\mathbf{BorelStoch}$ and $\mathbf{Gauss}$, false in $\mathbf{Stoch}$. See [[Conditionals and Disintegration]].
- **[[Bayesian Inversion]]**: conditioning a joint state built from a prior and a kernel in the other direction.
- **[[Conditional Independence]]** (Definitions 12.12, 12.16), stated purely with string diagrams; the semigraphoid axioms become theorems.
- **[[Almost-Sure Equality]]** (Definition 13.1): equality of kernels on the support of a state — the right notion of equality once inversions are involved.
- **Sufficient statistics**, Fisher–Neyman factorisation, Basu's and Bahadur's theorems are proved synthetically (Fritz §14–16).

## Markov categories are not enough for acausal models

Naturality of $\mathrm{del}$ forces normalisation, and normalisation forbids a **merge** $\mu : X \otimes X \to X$ ("these two wires carry the same value") — conditioning two independent random variables to be equal produces an unnormalised density. Markov categories therefore describe *directed* generative models (Bayesian networks, simulators, probabilistic programs) but not factor graphs or physical constraints, where variables are shared rather than passed. Two extensions:

| structure | copy | delete | merge | models |
|---|---|---|---|---|
| Markov category | yes | natural | no | directed, normalised |
| [[Partial Markov Category]] | yes | partial | via conditioning | observations, rejection |
| [[Hypergraph Category]] | yes | yes (not natural) | yes | relations, factor graphs, circuits |

[[Gaussian Relations]] form a hypergraph category containing $\mathbf{Gauss}$, which is the cleanest example of paying for merges with improper (unnormalised) objects.

## Lenticulum.jl

Lenticulum's directed layer (open models, pushforwards, inversions) lives in a Markov category; its factor-graph layer deliberately leaves it, because a variable node of degree three is a merge. See [Acausal Composition is a Hypergraph Category](https://mathstruct.org/Lenticulum.jl/dev/vault/Factor-Graphs/Acausal-Composition-is-a-Hypergraph-Category).

````tabs
tab: Julia
**Docs:** [Theories (Catlab): copy/delete — ThMonoidalCategoryWithDiagonals](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/theories/)
```julia
# FinStoch: stochastic matrices, rows = inputs; composition is the matrix product.
isstochastic(f) = all(≥(0), f) && all(sum(f, dims = 2) .≈ 1)
compose(f, g) = f * g                                    # Chapman–Kolmogorov
del(n) = ones(n, 1)                                      # X → I
prior = [0.3 0.7]                                        # a state I → X, X = {rain, dry}
sprinkler = [0.1 0.9; 0.6 0.4]                           # X → Y, Y = {on, off}
compose(prior, sprinkler)                                # pushforward f∗π = [0.45 0.55]
# naturality of del: every stochastic matrix f satisfies f ; del = del
isstochastic(sprinkler) && compose(sprinkler, del(2)) ≈ del(2)   # true
# the joint state I → X ⊗ Y: copy X, then apply f to one copy
joint = [prior[x] * sprinkler[x, y] for x in 1:2, y in 1:2]    # ψ(x, y)
vec(sum(joint, dims = 1)) ≈ vec(compose(prior, sprinkler))    # marginal = pushforward: true
```
tab: Lean
```lean
import Mathlib
open MeasureTheory ProbabilityTheory
-- Stoch in Mathlib: Markov kernels between measurable spaces
#check Kernel                        -- ProbabilityTheory.Kernel α β
#check IsMarkovKernel                -- every fibre is a probability measure (naturality of del)
#check @Kernel.comp                  -- composition of kernels (Chapman–Kolmogorov)
#check @Kernel.prod                  -- the tensor/pairing of kernels
#check @Kernel.deterministic         -- deterministic kernels from measurable functions
```
tab: Haskell
```haskell
-- FinStoch as the Kleisli category of a finite distribution monad
newtype Dist a = Dist { runDist :: [(a, Double)] }

instance Functor Dist where fmap f (Dist xs) = Dist [ (f x, p) | (x, p) <- xs ]
instance Applicative Dist where
  pure x = Dist [(x, 1)]
  Dist fs <*> Dist xs = Dist [ (f x, p * q) | (f, p) <- fs, (x, q) <- xs ]
instance Monad Dist where
  Dist xs >>= k = Dist [ (y, p * q) | (x, p) <- xs, (y, q) <- runDist (k x) ]

copy :: a -> Dist (a, a)
copy x = pure (x, x)                    -- deterministic
del :: a -> Dist ()
del _ = pure ()                         -- natural: fmap (const ()) of any Dist is pure ()

prob :: Eq a => a -> Dist a -> Double
prob a (Dist xs) = sum [ p | (x, p) <- xs, x == a ]
```
````
