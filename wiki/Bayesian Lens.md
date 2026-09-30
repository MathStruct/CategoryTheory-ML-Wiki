#definition #theorem #example

A **Bayesian lens** $(X, A) \to (Y, B)$ over a [[Markov Category]] (or [[Copy-Discard Category]]) $\mathcal C$ is a pair $(c, c')$ of

- a **forward** kernel $c : X \to Y$ — the generative model, likelihood or decoder;
- a **state-dependent backward** kernel $c' : \mathcal C(I, X) \to \mathcal C(B, A)$ — for each prior $\pi$ on $X$, a kernel $c'_\pi : B \to A$ (typically $A = X$, $B = Y$: an approximate posterior or encoder).

Composition runs forwards as usual and backwards **with the prior pushed forward**:

$$
(d, d') \circ (c, c') \;=\; \bigl(c \mathbin{;} d,\ \ \pi \mapsto d'_{c_* \pi} \mathbin{;} c'_\pi\bigr).
$$

The pair is **exact** when $c'_\pi = c^\dagger_\pi$ is the [[Bayesian Inversion]] of $c$ at $\pi$. The backward kernel is indexed by the prior exactly as a lens's put is indexed by the forward input: *the prior plays the role of the linearisation point*.

> Sources: St Clere Smithe, *Bayesian Updates Compose Optically* [arXiv:2006.01631](https://arxiv.org/abs/2006.01631) ([[Bayesian Updates Compose Optically|notes]]) Definitions 3.1, 3.4, 4.3, Propositions 4.4–4.5, Theorem 5.2, Corollary 5.3; Braithwaite, Hedges & St Clere Smithe, *The Compositional Structure of Bayesian Inference* [arXiv:2305.06112](https://arxiv.org/abs/2305.06112) ([[The Compositional Structure of Bayesian Inference|notes]]) Definitions 8–9, Remark 10, Proposition 11, Definitions 13, 15, Theorem 20; Braithwaite & Hedges, *Dependent Bayesian Lenses* [arXiv:2209.14728](https://arxiv.org/abs/2209.14728) ([[Dependent Bayesian Lenses|notes]]); St Clere Smithe [arXiv:2109.04461](https://arxiv.org/abs/2109.04461) ([[Compositional Active Inference I - Bayesian Lenses and Statistical Games|notes]]) Definitions 3.7, 3.13, Theorem 3.14; St Clere Smithe & Perin, *AutoBayes* [arXiv:2503.18608](https://arxiv.org/abs/2503.18608) ([[AutoBayes - A Compositional Framework for Generalized Variational Inference|notes]]) Definitions 9–16, Theorem 13.

## Three equivalent constructions

1. **Grothendieck lenses** (Braithwaite et al. Definitions 8–9; St Clere Smithe Definition 3.4). Let $\mathbf{Stat} : \mathcal C^{\mathrm{op}} \to \mathbf{Cat}$ send $X$ to the category whose morphisms $A \to B$ are *functions* $\mathcal C(I, X) \to \mathcal C(A, B)$ ("state-indexed kernels"), with reindexing by pushforward. Then $\mathbf{BLens}(\mathcal C) = \int \mathbf{Stat}^{\mathrm{op}}$ — the [[Grothendieck Construction]] of the fibrewise opposite. For $\mathcal C = \mathbf{Set}$ (degenerately Markov) $f^\sharp : \mathcal C(1, X) \to \mathcal C(B, A)$ is a function $X \times B \to A$, recovering ordinary [[Lens|lenses]] (Remark 10).
2. **Optics** (St Clere Smithe Definition 4.3, Proposition 4.5). $\mathbf{BayesLens}$ is a category of [[Optic|optics]] for an action on presheaves; the stochastic residual cannot be reduced to a copy of the input, which is why Bayesian updates "compose optically" rather than as cartesian lenses.
3. **With latent spaces** (AutoBayes Definitions 9–12). Replace kernels by [[Open Model|open models]] $c : X \rightsquigarrow [\![c]\!] \times Y$; the backward part $c'_\pi : Y \rightsquigarrow X \times [\![c]\!]$ reconstructs the latent space too, and composition needs **no integration**.

## The chain rule: Bayesian inversion is functorial

> **Theorem** (St Clere Smithe 5.2; Braithwaite et al. Proposition 11; AutoBayes Theorem 13). If $(c, c^\dagger)$ and $(d, d^\dagger)$ are exact, their composite is exact: $(c \mathbin{;} d)^\dagger_\pi = d^\dagger_{c_*\pi} \mathbin{;} c^\dagger_\pi$, up to [[Almost-Sure Equality|almost-sure equality]]. Hence Bayesian inversion is a functor $\mathcal C \to \mathbf{BLens}(\mathcal C)$ — a section of the lens fibration, exact on supports (Braithwaite et al. Theorem 20).

The practical content: attach an **approximate** backward kernel to each part of a model locally; the composite is automatically a *correctly structured* approximate posterior for the whole model, even though it is not the exact one. This is "define an `rrule` per primitive and let the AD system compose them", for inference.

## The approximate inversion is a free choice

$c'$ need not equal $c^\dagger$. Any kernel of the right type is a Bayesian lens:

| choice of $c'_\pi$ | method |
|---|---|
| $c^\dagger_\pi$ exactly | conjugate models, exact message passing |
| an encoder network $q_\phi(x \mid y)$ | amortised variational inference (VAE) |
| a Gaussian with learned mean/covariance | Laplace, mean-field VI |
| a particle set | sequential Monte Carlo |
| a solver's fixed point | implicit / equilibrium models |
| a denoising diffusion posterior | diffusion-based inverse problems |

How good the choice is, is measured by a divergence to $c^\dagger_\pi$, which cannot be computed directly; the [[Variational Free Energy]] bounds it, and [[Statistical Game|statistical games]] carry that loss compositionally.

## Parallel composition is lax

The tensor of two Bayesian lenses can only feed each backward kernel the **marginal** of a joint prior (AutoBayes Definition 15, Remark 16): $(c \otimes d)^\dagger \neq c^\dagger \otimes d^\dagger$ when the prior correlates the two inputs. Inversion is only a **lax monoidal** functor ([[Lax Functor]]), and for Shannon entropies the defect is the mutual information (Remark 26) — the formal content of "mean-field is wrong, and by exactly this much".

## Lenticulum.jl

A Lenticulum factor carries an inversion alongside its forward kernel (`BayesianLens`, `invert`, `ExactInversion`, `AmortisedInversion`, `SolverInversion`, `ProximalInversion`). See [Inversions and Bayesian Lenses](https://mathstruct.org/Lenticulum.jl/dev/vault/Foundations/Inversions-and-Bayesian-Lenses).

````tabs
tab: Julia
**Docs:** [Theories (Catlab): copy/delete — ThMonoidalCategoryWithDiagonals](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/theories/)
```julia
# Bayesian lenses on FinStoch: forward = stochastic matrix, backward = prior ↦ kernel.
struct BLens{F,B}; fwd::F; bwd::B; end
pushforward(π, c) = vec(π' * c)
bayes(c) = π -> (q = pushforward(π, c);
                 [q[y] > 0 ? c[x, y] * π[x] / q[y] : 1 / length(π) for y in axes(c, 2), x in eachindex(π)])
exact(c) = BLens(c, bayes(c))
compose(l2::BLens, l1::BLens) = BLens(l1.fwd * l2.fwd, π -> l2.bwd(pushforward(π, l1.fwd)) * l1.bwd(π))
c = [0.9 0.1; 0.2 0.8]; d = [0.7 0.3; 0.4 0.6]; π = [0.25, 0.75]
composite = compose(exact(d), exact(c))
composite.bwd(π) ≈ bayes(c * d)(π)                # the chain rule: composite of exact = exact: true
# an approximate lens: a backward pass that ignores the prior (a fixed "encoder")
enc = BLens(c, π -> [0.8 0.2; 0.3 0.7])
compose(exact(d), enc).bwd(π) ≈ bayes(c * d)(π)   # false: structured, but not exact
```
tab: Lean
```lean
import Mathlib
-- Bayesian lenses over a category whose hom-types are `Hom` and whose states are `Hom Unit X`
structure BLens (Hom : Type → Type → Type) (X A Y B : Type) where
  fwd : Hom X Y
  bwd : Hom Unit X → Hom B A          -- prior ↦ backward kernel

def BLens.comp {Hom : Type → Type → Type} (comp : ∀ {P Q R}, Hom P Q → Hom Q R → Hom P R)
    {X A Y B Z C : Type} (l₁ : BLens Hom X A Y B) (l₂ : BLens Hom Y B Z C) : BLens Hom X A Z C where
  fwd := comp l₁.fwd l₂.fwd
  bwd := fun π => comp (l₂.bwd (comp π l₁.fwd)) (l₁.bwd π)   -- d'_{c∗π} ; c'_π
```
tab: Haskell
```haskell
-- Bayesian lenses over finite distributions
newtype Dist a = Dist { runDist :: [(a, Double)] }
type Kernel a b = a -> Dist b

data BLens x y = BLens { fwd :: Kernel x y, bwd :: Dist x -> Kernel y x }

push :: Dist x -> Kernel x y -> Dist y
push (Dist ps) k = Dist [ (y, p * q) | (x, p) <- ps, (y, q) <- runDist (k x) ]

compose :: BLens y z -> BLens x y -> BLens x z
compose (BLens d d') (BLens c c') =
  BLens (\x -> push (c x) d) (\prior z -> push (d' (push prior c) z) (c' prior))
```
````
