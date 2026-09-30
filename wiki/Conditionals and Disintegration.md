#definition #theorem #example

Let $\mathcal C$ be a [[Markov Category]]. A **conditional** of $f : A \to X \otimes Y$ with respect to $X$ is a morphism $f_{|X} : X \otimes A \to Y$ such that $f$ can be recovered by first producing $X$ (the marginal $f_X$) and then producing $Y$ from $X$ and the input:

$$
f \;=\; A \xrightarrow{\ \mathrm{copy}\ } A \otimes A \xrightarrow{\ f_X \otimes 1\ } X \otimes A \xrightarrow{\ \mathrm{copy}_X \otimes 1\ } X \otimes X \otimes A \xrightarrow{\ 1 \otimes f_{|X}\ } X \otimes Y
$$

(Fritz, Definition 11.5; drawn as a string diagram it is "draw $x$, keep a copy, feed the other copy to $f_{|X}$"). For a **state** $\psi : I \to X \otimes Y$ this is a **disintegration**: $\psi = (\psi_X, \psi_{|X})$, a marginal together with a conditional kernel $X \to Y$ (Definition 11.1; Cho & Jacobs Definition 3.5). $\mathcal C$ **has conditionals** if every such $f$ admits one.

> Sources: Fritz [arXiv:1908.07021](https://arxiv.org/abs/1908.07021) ([[A Synthetic Approach to Markov Kernels, Conditional Independence and Theorems on Sufficient Statistics|notes]]) Definitions 11.1, 11.5, Examples 11.2–11.8, Lemmas 11.11–11.12, Remark 11.13, Propositions 11.15, 11.17, Definitions 11.22, 11.31, Lemma 11.24, Proposition 11.34; Cho & Jacobs [arXiv:1709.00322](https://arxiv.org/abs/1709.00322) ([[Disintegration and Bayesian Inversion via String Diagrams|notes]]) §3 (Definition 3.5, Examples 3.6–3.9, Proposition 3.10, Theorem 3.11), §7; Chang & Pollard (1997), *Conditioning as disintegration*.

## Examples

- **$\mathbf{FinStoch}$** has conditionals: $\psi_{|X}(y \mid x) = \psi(x, y) / \psi_X(x)$ where $\psi_X(x) > 0$, anything elsewhere (Examples 11.2, 11.6).
- **$\mathbf{BorelStoch}$** has conditionals — the classical disintegration theorem for standard Borel spaces (Example 11.7; Cho & Jacobs Theorem 3.11).
- **$\mathbf{Gauss}$** has conditionals: the Schur-complement formulas for conditioning a joint Gaussian (Example 11.8).
- **$\mathbf{Stoch}$** does *not*: there are joint measures on non-standard spaces with no regular conditional probability (Example 11.3). This is one reason the synthetic theory is stated axiomatically.

## Consequences of having conditionals

- **Uniqueness up to almost-sure equality** (Proposition 11.15): any two conditionals agree $\psi_X$-a.s. ([[Almost-Sure Equality]]).
- **Disintegration / Bayes** (Proposition 11.17): for $p : A \to X$ and $f : X \to Y$ there is $s : A \otimes Y \to X$ reversing $f$ relative to $p$ — this is [[Bayesian Inversion]] in its parametrised form.
- **Conditioning commutes with marginalisation** (Remark 11.13), and conditionals of conditionals are conditionals (Lemma 11.11) — the algebraic backbone of the chain rule of probability, $p(x, y, z) = p(x)\,p(y \mid x)\,p(z \mid x, y)$.
- **Positivity and causality** (Lemma 11.24, Proposition 11.34): categories with conditionals are *positive* and *causal*, properties needed for conditional independence to behave (semigraphoid laws).

## The computational reading

A disintegration trades a joint object for a marginal plus a kernel — exactly what a **factorised** probabilistic model, an **autoregressive** model, or a **Bayesian network** stores. Computing a conditional is the expensive step of inference: in $\mathbf{FinStoch}$ it is division by a marginal (a sum over the other variables); in general it is the intractable part that variational methods approximate by a *chosen* kernel of the right type ([[Bayesian Lens]], [[Variational Free Energy]]).

````tabs
tab: Julia
**Docs:** [Theories (Catlab): copy/delete — ThMonoidalCategoryWithDiagonals](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/theories/)
```julia
# Disintegrating a joint state ψ on X × Y in FinStoch: ψ = (ψ_X, ψ_{|X}).
ψ = [0.10 0.20 0.10;
     0.30 0.00 0.30]                                  # rows x ∈ {1,2}, columns y ∈ {1,2,3}
ψX = vec(sum(ψ, dims = 2))                             # marginal [0.4, 0.6]
cond = ψ ./ ψX                                         # ψ_{|X}(y | x), rows sum to 1
recombined = [ψX[x] * cond[x, y] for x in 1:2, y in 1:3]
recombined ≈ ψ                                         # the defining equation: true
# conditionals are unique only ψX-a.s.: modify the kernel where ψX(x) = 0 and nothing changes
ψ0 = [0.5 0.5; 0.0 0.0]; c1 = [0.5 0.5; 1.0 0.0]; c2 = [0.5 0.5; 0.0 1.0]
all(vec(sum(ψ0, dims = 2)) .* c ≈ ψ0 for c in (c1, c2))   # both are conditionals: true
```
tab: Lean
```lean
import Mathlib
open MeasureTheory ProbabilityTheory
-- Mathlib's disintegration of a finite measure on a product with a standard Borel factor:
#check @Measure.condKernel              -- ρ.condKernel : Kernel α Ω
#check @Measure.compProd_fst_condKernel -- ρ.fst ⊗ₘ ρ.condKernel = ρ
#check @condDistrib                     -- the conditional distribution of Y given X
```
tab: Haskell
```haskell
import qualified Data.Map as M

-- disintegrate a finite joint distribution into a marginal and a conditional kernel
type Joint x y = M.Map (x, y) Double

marginal :: Ord x => Joint x y -> M.Map x Double
marginal j = M.fromListWith (+) [ (x, p) | ((x, _), p) <- M.toList j ]

conditional :: (Ord x, Eq x) => Joint x y -> x -> [(y, Double)]
conditional j x = [ (y, p / px) | ((x', y), p) <- M.toList j, x' == x, px > 0 ]
  where px = M.findWithDefault 0 x (marginal j)
```
````
