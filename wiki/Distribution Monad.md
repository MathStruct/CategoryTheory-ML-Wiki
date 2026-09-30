#definition #example #program

The (finitary) **distribution monad** $\mathrm{Dist}$ on $\mathbf{Set}$ sends $X$ to the set of **finitely supported probability distributions** on $X$ — functions $p : X \to [0, 1]$ that are nonzero on finitely many points and satisfy $\sum_x p(x) = 1$, written as formal convex combinations $\sum_i p_i\, x_i$. A function $f : X \to Y$ acts by pushing forward, $\mathrm{Dist}(f)(p)(y) = \sum_{f(x) = y} p(x)$. The monad structure is (CTfS §5.3.4.2)

$$
\eta_X(x) = 1\,x \ \ (\text{the Kronecker delta } \delta_x), \qquad \mu_X\Big(\sum_i q_i\, p_i\Big) = \sum_i q_i\, p_i \ \ (\text{the weighted average of distributions}).
$$

It is the monad of **probability**: a Kleisli arrow $X \to \mathrm{Dist}(Y)$ is a *stochastic map* (a Markov kernel), and Kleisli composition sums over the intermediate outcomes — the Chapman–Kolmogorov equation.

> Sources: CTfS §5.3.4 (5.3.4.2, Example 5.3.4.3, Application 5.3.4.4); [[Kleisli Category]], [[Monad]].

## Kleisli arrows are stochastic matrices

For finite $X, Y$, a Kleisli arrow $f : X \to \mathrm{Dist}(Y)$ is an $|X| \times |Y|$ matrix with nonnegative entries and rows summing to 1 (row $x$ is the distribution $f(x)$). The Kleisli composite of $f : X \to \mathrm{Dist}(Y)$ and $g : Y \to \mathrm{Dist}(Z)$ is

$$
(g \circ_{\mathrm{Dist}} f)(x)(z) = \sum_{y} f(x)(y)\, g(y)(z),
$$

the matrix product $F G$, and $\eta$ is the identity matrix. So $\mathrm{Kl}(\mathrm{Dist})$ restricted to finite sets is the category of stochastic matrices — the probabilistic analogue of $\mathrm{Kl}(\mathcal P) = \mathbf{Rel}$ with Boolean matrices ([[Power Set Monad]]).

## Examples

- **A star and a detector** (CTfS Application 5.3.4.4): a star emits photons of various wavelengths with certain probabilities, $\{\ast\} \to \mathrm{Dist}(W)$; a material absorbs a photon of wavelength $w$ and emits an electron with an energy distribution depending on $w$, $W \to \mathrm{Dist}(E)$. The Kleisli composite $\{\ast\} \to \mathrm{Dist}(E)$ is the observed energy spectrum — a weighted sum over wavelengths, exactly $\mu$.
- **Markov chains** (CTfS Example 5.3.4.3): a Kleisli endomorphism $S \to \mathrm{Dist}(S)$, i.e. a $\mathrm{Dist}$-valued instance on the one-loop schema — see [[Markov Chain]].
- **Noisy channels** in information theory, **mixed strategies** in game theory, **probabilistic programs** — all are Kleisli arrows for $\mathrm{Dist}$.
- A distribution on a product, $\mathrm{Dist}(X \times Y)$, is a joint distribution; the monad's *strength* $X \times \mathrm{Dist}(Y) \to \mathrm{Dist}(X \times Y)$ makes $\mathrm{Kl}(\mathrm{Dist})$ a monoidal category — the starting point of Markov categories and categorical probability.

The algebras of $\mathrm{Dist}$ are **convex sets** (sets where convex combinations can be evaluated, like $[0, 1]$ or any vector space's convex subsets): expectation $\mathrm{Dist}(\mathbb R) \to \mathbb R$ is such an algebra structure. For measure-theoretic probability the analogous monad on measurable spaces is the Giry monad.

## The general picture

The distribution monad is the finitely supported case of the [[Giry Monad]]; its Kleisli category is the discrete part of $\mathbf{Stoch}$, the prototypical [[Markov Category]]. There, conditioning, [[Bayesian Inversion]] and [[Conditional Independence]] become equations between string diagrams.

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# finitary distributions as Dict(outcome => probability)
η(x) = Dict(x => 1.0)
function μ(pp)                               # a distribution over distributions ↦ weighted average
  out = Dict{Any,Float64}()
  for (p, q) in pp, (x, w) in p
    out[x] = get(out, x, 0.0) + q * w
  end
  out
end
bind(p, f) = μ([f(x) => w for (x, w) in p])          # Kleisli extension
# star → wavelength → electron energy (CTfS Application 5.3.4.4), made-up numbers
star = Dict(:red => 0.7, :blue => 0.3)
absorb(w) = w == :red ? Dict(:low => 0.9, :high => 0.1) : Dict(:low => 0.2, :high => 0.8)
spectrum = bind(star, absorb)                 # :low => 0.69, :high => 0.31
bind(star, η) == star                         # unit law
```
tab: Lean
```lean
import Mathlib
-- Mathlib's finitely supported probability mass functions form a monad
#check PMF                         -- PMF α: α → ℝ≥0∞ summing to 1
#check (inferInstance : Monad PMF)
#check @PMF.pure                   -- the Dirac delta η
#check @PMF.bind                   -- Kleisli extension: weighted sum
#check @MeasureTheory.Measure.bind -- the Giry monad on measures
```
tab: Haskell
```haskell
import qualified Data.Map as Map

newtype Dist a = Dist { runDist :: [(a, Double)] }     -- weighted outcomes (not normalised merge)

instance Functor Dist where fmap f (Dist xs) = Dist [ (f x, p) | (x, p) <- xs ]
instance Applicative Dist where
  pure x = Dist [(x, 1)]                               -- η: the Dirac distribution
  Dist fs <*> Dist xs = Dist [ (f x, p * q) | (f, p) <- fs, (x, q) <- xs ]
instance Monad Dist where
  Dist xs >>= k = Dist [ (y, p * q) | (x, p) <- xs, (y, q) <- runDist (k x) ]   -- μ: weighted sum

collect :: Ord a => Dist a -> Map.Map a Double
collect (Dist xs) = Map.fromListWith (+) xs

data Colour = Red | Blue deriving (Eq, Ord, Show)
data Energy = Low | High deriving (Eq, Ord, Show)
star :: Dist Colour
star = Dist [(Red, 0.7), (Blue, 0.3)]
absorb :: Colour -> Dist Energy
absorb Red  = Dist [(Low, 0.9), (High, 0.1)]
absorb Blue = Dist [(Low, 0.2), (High, 0.8)]
-- collect (star >>= absorb) == fromList [(Low,0.69),(High,0.31)] (up to rounding)
```
````
