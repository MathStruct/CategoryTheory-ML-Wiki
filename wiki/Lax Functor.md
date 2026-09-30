#definition #example

A **lax functor** $F : \mathcal B \to \mathcal C$ between [[Bicategory|bicategories]] maps objects to objects, 1-cells to 1-cells and 2-cells to 2-cells, but preserves composition and identities only **up to a comparison 2-cell that need not be invertible**:

$$
\phi_{g,f} : F g \circ F f \Longrightarrow F(g \circ f),
\qquad
\phi_A : 1_{FA} \Longrightarrow F(1_A),
$$

natural in $f, g$ and coherent with the associators and unitors. If the comparison cells point the other way ($F(g \circ f) \Rightarrow Fg \circ Ff$) the functor is **oplax** (colax); if they are invertible it is a **pseudofunctor**; if they are identities it is **strict**. The same pattern one level down gives **lax monoidal functors** $F : (\mathcal C, \otimes) \to (\mathcal D, \otimes)$ with $\mu_{A,B} : FA \otimes FB \to F(A \otimes B)$ and $\eta : I \to FI$ — see [[Monoidal Functor]].

> Sources: Bénabou (1967); Johnson & Yau, *2-Dimensional Categories* ch. 4; St Clere Smithe & Perin, *AutoBayes* [arXiv:2503.18608](https://arxiv.org/abs/2503.18608) ([[AutoBayes - A Compositional Framework for Generalized Variational Inference|notes]]) Theorem 13, Remark 16, Remark 30 and footnote 3; Braithwaite, Hedges & St Clere Smithe [arXiv:2305.06112](https://arxiv.org/abs/2305.06112) ([[The Compositional Structure of Bayesian Inference|notes]]) Proposition 11; DaoFP §17 (lax monoidal functors).

## "Lax" as a measure of how much structure a construction destroys

In categorical machine learning the word *lax* is not a technicality: it names **exactly the approximation an algorithm makes**, and the comparison cell is the error term. Three instances from the AutoBayes paper:

| construction | preserves | laxness | what the comparison cell measures |
|---|---|---|---|
| Bayesian inversion $(-)^\dagger$ on open models | sequential composition (Theorem 13) | a **pseudo**functor, and only *almost surely* (footnote 3) | nothing — the chain rule is exact up to null sets |
| $(-)^\dagger$ and the tensor $\otimes$ | — | **lax monoidal** (Remark 16) | the correlation between parallel branches thrown away: $(c \otimes d)^\dagger \ne c^\dagger \otimes d^\dagger$; for Shannon entropies it is the **mutual information** (Remark 26) |
| assigning gradients to parameterized statistical games | — | a **lax section** of a fibration (Remark 30) | the off-diagonal Jacobian blocks dropped by composing gradients locally (Definition 29) — i.e. "do you backprop through the sampler?" |

The moral carried into implementations: *laxness is a number you can compute and report*. The mean-field approximation of variational inference is the lax monoidality of Bayesian inversion; a stop-gradient on a sampling path is the laxness of the gradient section.

## Examples

- A **monad** is a lax functor $\mathbf 1 \to \mathbf{Cat}$ (Bénabou): the single object goes to a category $\mathcal C$, the identity 1-cell to $T$, and the comparison cells are $\mu : TT \Rightarrow T$ and $\eta : 1 \Rightarrow T$ ([[Monad]]).
- A **pseudofunctor** $\mathcal C^{\mathrm{op}} \to \mathbf{Cat}$ is an indexed category; the [[Grothendieck Construction]] turns it into a fibration. Reindexing along a composite agrees with the composite of reindexings only up to isomorphism — hence *pseudo*.
- The **power set** functor $\mathcal P : (\mathbf{Set}, \times) \to (\mathbf{Set}, \times)$ is lax monoidal with $\mu(S, T) = S \times T$: every pair of subsets gives a "rectangular" subset of the product, but most subsets of $A \times B$ are not rectangles, so $\mu$ is not invertible ([[Power Set Monad]]). The same comparison for the [[Distribution Monad]] sends two distributions to their *independent* joint — the correlations a lax structure cannot see.
- **Applicative functors** in Haskell are lax monoidal endofunctors of $(\mathbf{Hask}, \times)$ ([[Applicative Functor]]).
- **Backpropagation** is a *strict* functor $\mathbf{Para}(\mathcal C) \to \mathbf{Para}(\mathbf{Lens}(\mathcal C))$ ([[Reverse Derivative Category]]): the reverse chain rule holds on the nose, and this is why autodiff is exact.

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# Laxness of Bayesian inversion w.r.t. ⊗, on finite spaces.
# c : X → Y and d : X′ → Y′ are identity channels; the prior ω on X × X′ is correlated.
ω = [0.4 0.1; 0.1 0.4]                          # joint prior on X × X′ (2×2)
ωX, ωX′ = vec(sum(ω, dims = 2)), vec(sum(ω, dims = 1))
mutual_information(ω) = sum(ω[i, j] * log(ω[i, j] / (sum(ω[i, :]) * sum(ω[:, j])))
                            for i in axes(ω, 1), j in axes(ω, 2) if ω[i, j] > 0)
# the parallel composite of the two inversions only sees the marginals ωX, ωX′:
product_of_marginals = ωX * ωX′'
round(mutual_information(ω); digits = 4)        # 0.1927 nats: the size of the lax comparison
product_of_marginals ≈ ω                        # false — (c ⊗ d)† ≠ c† ⊗ d† here
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
#check Pseudofunctor          -- functors between bicategories up to coherent isomorphism
#check LaxFunctor             -- comparison 2-cells F g ≫ F f ⟶ F (g ≫ f), not invertible
#check OplaxFunctor
#check Functor.LaxMonoidal    -- μ : F X ⊗ F Y ⟶ F (X ⊗ Y), ε : 𝟙_ ⟶ F (𝟙_)
```
tab: Haskell
```haskell
-- A lax monoidal functor on (Hask, (,), ()): the comparison maps of an Applicative.
class Functor f => Monoidal f where
  unit  :: f ()
  (>*<) :: f a -> f b -> f (a, b)          -- μ : F a ⊗ F b → F (a ⊗ b), not invertible in general

instance Monoidal [] where
  unit = [()]
  xs >*< ys = [ (x, y) | x <- xs, y <- ys ]

-- μ is not invertible: [(1,'a'),(2,'b')] :: [(Int, Char)] is not of the form xs >*< ys
```
````
