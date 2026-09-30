#definition #example

A **copy-discard category** (**CD category**, also *gs-monoidal category*) is a [[Symmetric Monoidal Category]] $(\mathcal C, \otimes, I)$ in which every object $X$ carries a commutative comonoid

$$
\mathrm{copy}_X : X \to X \otimes X, \qquad \mathrm{del}_X : X \to I,
$$

compatible with the monoidal structure ($\mathrm{copy}_{X \otimes Y}$ and $\mathrm{del}_{X \otimes Y}$ are built from those of $X$ and $Y$, and $\mathrm{del}_I = \mathrm{copy}_I = \mathrm{id}_I$ up to coherence). Morphisms are *not* required to preserve this structure. A morphism $f$ is

- **causal** / **discardable** if $f \mathbin{;} \mathrm{del} = \mathrm{del}$ (deleting the output is the same as deleting the input — "$f$ is normalised");
- **deterministic** if $f \mathbin{;} \mathrm{copy} = \mathrm{copy} \mathbin{;} (f \otimes f)$ (copying the output equals running $f$ twice on a copied input).

A CD category in which every morphism is causal — equivalently, $I$ is terminal — is **affine**, and that is exactly a [[Markov Category]].

> Sources: Cho & Jacobs, *Disintegration and Bayesian Inversion via String Diagrams* [arXiv:1709.00322](https://arxiv.org/abs/1709.00322) ([[Disintegration and Bayesian Inversion via String Diagrams|notes]]) Definitions 2.2–2.3; Fritz [arXiv:1908.07021](https://arxiv.org/abs/1908.07021) ([[A Synthetic Approach to Markov Kernels, Conditional Independence and Theorems on Sufficient Statistics|notes]]) Definitions 2.1, 10.1; Corradini & Gadducci (1999), *An algebraic presentation of term graphs via gs-monoidal categories*; Carboni & Walters (1987), cartesian bicategories; St Clere Smithe [arXiv:2109.04461](https://arxiv.org/abs/2109.04461) ([[Compositional Active Inference I - Bayesian Lenses and Statistical Games|notes]]) Example 2.21.

## Examples

| CD category | morphisms $X \to Y$ | causal = | deterministic = |
|---|---|---|---|
| $\mathbf{Set}$ ($\otimes = \times$) | functions | all | all (it is cartesian) |
| $\mathbf{Rel}$ | relations | total relations | single-valued (partial) functions |
| $\mathbf{Mat}(\mathbb R_{\ge0})$ | non-negative matrices, i.e. **unnormalised** kernels | stochastic matrices | 0/1 matrices with one 1 per row |
| $\mathbf{sfKrn}$ | s-finite kernels (St Clere Smithe Ex. 2.21) | Markov kernels | kernels of measurable functions |
| Kleisli of a commutative monad $T$ | $X \to TY$ | when $T$ is affine ($T1 \cong 1$) | "pure" maps $\eta \circ f$ |

In $\mathbf{Mat}(\mathbb R_{\ge 0})$ a morphism $f : X \to Y$ is a matrix $f(y \mid x) \ge 0$; $\mathrm{del}_X$ is the all-ones row-sum map, so $f$ is causal iff every row sums to $1$. **Normalisation is causality.**

## Why the distinction matters

- **Copying is not natural.** In a cartesian category every map commutes with copying; in a CD category only the deterministic ones do. Flipping a coin and copying the result is not the same as flipping two coins. This single failure is what makes string diagrams for probability differ from those for functions: a copy node must be drawn explicitly, and "reusing a variable" is a genuine operation.
- **Deleting is natural exactly when everything is normalised.** Dropping that requirement (allowing unnormalised measures, likelihoods, energies) lets one condition, observe and *merge* wires — at the price of tracking normalising constants separately. See [[Partial Markov Category]] and [[Hypergraph Category]] for the two standard ways to add conditioning, and [[Gaussian Relations]] for a worked example.
- **The deterministic morphisms form a cartesian subcategory** $\mathcal C_{\mathrm{det}}$ (Fritz Remark 10.13): copy and delete are natural there, so the monoidal product is a product.

````tabs
tab: Julia
**Docs:** [Theories (Catlab): copy/delete — ThMonoidalCategoryWithDiagonals](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/theories/)
```julia
# Mat(ℝ≥0) as a CD category: morphisms X → Y are |X|×|Y| non-negative matrices (row = input).
copy(n) = [i == j == k ? 1.0 : 0.0 for i in 1:n, j in 1:n, k in 1:n] |> A -> reshape(A, n, n * n)
del(n) = ones(n, 1)
iscausal(f) = f * del(size(f, 2)) ≈ del(size(f, 1))            # rows sum to 1
isdeterministic(f) = f * copy(size(f, 2)) ≈ copy(size(f, 1)) * kron(f, f)
coin = [0.5 0.5]                                                # a state 1 → 2
flip = [0.0 1.0; 1.0 0.0]                                       # a deterministic map 2 → 2
iscausal(coin), isdeterministic(coin)                           # (true, false): randomness
iscausal(flip), isdeterministic(flip)                           # (true, true)
iscausal([0.9 0.3; 0.2 0.4])                                    # false: unnormalised
```
tab: Lean
```lean
import Mathlib
open CategoryTheory MonoidalCategory
-- A copy-discard structure: a commutative comonoid on every object (compatibility with ⊗ omitted).
class CopyDiscard (C : Type*) [Category C] [MonoidalCategory C] [SymmetricCategory C] where
  copy : ∀ X : C, X ⟶ X ⊗ X
  del : ∀ X : C, X ⟶ 𝟙_ C
  -- counit, coassociativity and commutativity laws omitted

def Causal {C : Type*} [Category C] [MonoidalCategory C] [SymmetricCategory C] [CopyDiscard C]
    {X Y : C} (f : X ⟶ Y) : Prop :=
  f ≫ CopyDiscard.del Y = CopyDiscard.del X
```
tab: Haskell
```haskell
-- Finite unnormalised kernels: a morphism x -> y is a function x -> [(y, weight)]
type Kernel x y = x -> [(y, Double)]

del :: Kernel x ()
del x = [((), 1)]

(>=>) :: Kernel x y -> Kernel y z -> Kernel x z
(f >=> g) x = [ (z, w * v) | (y, w) <- f x, (z, v) <- g y ]

causal :: [x] -> Kernel x y -> Bool           -- deleting the output = deleting the input
causal xs f = and [ abs (sum (map snd ((f >=> del) x)) - 1) < 1e-12 | x <- xs ]

coin :: Kernel () Bool
coin () = [(True, 0.5), (False, 0.5)]
-- causal [()] coin == True
```
````
