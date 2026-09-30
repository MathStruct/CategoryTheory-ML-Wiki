#definition #example

For a [[Monoidal Category]] $(\mathcal M, \otimes, J)$, an **$\mathcal M$-actegory** is a category $\mathcal C$ with a functor (the *action*)

$$
\bullet : \mathcal M \times \mathcal C \longrightarrow \mathcal C
$$

and natural isomorphisms $\varepsilon_X : J \bullet X \cong X$ and $\delta_{M,N,X} : (M \otimes N) \bullet X \cong M \bullet (N \bullet X)$ satisfying the coherence laws of a monoid action — pentagon- and triangle-shaped diagrams. The spelling is deliberate: an actegory is a *vertical categorification of a [[Monoid Action]]*, with the monoid replaced by a monoidal category and the action equations by coherent isomorphisms.

> Sources: Capucci & Gavranović, *Actegories for the Working Amthematician* [arXiv:2203.16351](https://arxiv.org/abs/2203.16351) ([[Actegories for the Working Amthematician|notes]]); Capucci, Gavranović, Hedges & Fjeldgren Rischel, *Towards Foundations of Categorical Cybernetics* [arXiv:2105.06332](https://arxiv.org/abs/2105.06332) ([[Towards Foundations of Categorical Cybernetics|notes]]) Definitions 1, 6, Proposition 5; Riley, *Categories of Optics* [arXiv:1809.00738](https://arxiv.org/abs/1809.00738) ([[Categories of Optics|notes]]); Janelidze & Kelly, *A note on actions of a monoidal category*, TAC 9 (2001).

## Why it matters: parameters and residuals act on data

In categorical cybernetics $\mathcal M$ is a category of **parameters** (or of **residuals**, **contexts**, **states**) and $\bullet$ says how a parameter is attached to a piece of data. Two constructions are defined over an actegory rather than over a monoidal category:

- **[[Para Construction|$\mathbf{Para}_\bullet(\mathcal C)$]]** — morphisms $X \to Y$ are pairs $(M, \varphi : M \bullet X \to Y)$. Taking $\mathcal C = \mathcal M$ acting on itself by $\otimes$ recovers the familiar $\mathbf{Para}(\mathcal C)$ of Cruttwell et al. Capucci et al. (Proposition 5) prove $\mathbf{Para}(-)$ is a **monad on $\mathcal M$-actegories**: a twice-parametrised map is a once-parametrised one with parameter $M \otimes N$.
- **[[Optic|Mixed optics]]** — a map $(A, A') \to (B, B')$ is an element of $\int^{M} \mathcal C(A, M \bullet B) \times \mathcal D(M \bullet' B', A')$: the forward pass stores a *residual* $M$ which the backward pass consumes. Different actions give lenses (cartesian product), prisms (coproduct), grates, Kleisli optics, …

Separating the category of parameters from the category of data is what allows, for example, a *discrete* parameter space acting on smooth maps, or a probabilistic residual acting on deterministic data.

## Examples

- Every monoidal category acts on itself: $M \bullet X = M \otimes X$ (the **regular** actegory).
- $\mathbf{Set}$ acts on any category with coproducts by *copowers*: $S \bullet X = \coprod_{s \in S} X$.
- A **strong functor** / strong monad is a functor between actegories commuting with the actions up to coherent maps; the [[Functorial Strength|strength]] $A \otimes TB \to T(A \otimes B)$ of a monad is exactly this ([[Monad]]).
- $\mathbf{Smooth}$ acting on itself by $\times$ gives $\mathbf{Para}(\mathbf{Smooth})$, the home of neural network layers ([[Para Construction]]).
- A symmetric monoidal category acting on its Kleisli category of a commutative monad — e.g. deterministic parameters acting on stochastic maps.

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# An actegory in miniature: the monoidal category (types, ×) acting on vectors by "tagging".
# Action on objects: M • X = (M, X); on morphisms: (m, f) ↦ (p, x) -> (m(p), f(x)).
act(m, f) = ((p, x),) -> (m(p), f(x))
# coherence: (M ⊗ N) • X ≅ M • (N • X), realised by reassociation
δ(((m, n), x)) = (m, (n, x))
ε((_, x)) = x                                     # J • X ≅ X with J the unit type
v = ((:lr, :momentum), [1.0, 2.0])
δ(v) == (:lr, (:momentum, [1.0, 2.0]))            # true
ε((nothing, [1.0, 2.0])) == [1.0, 2.0]            # true
act(p -> p + 1, x -> 2x)((1, 3.0)) == (2, 6.0)    # functoriality of • on a pair of maps: true
```
tab: Lean
```lean
import Mathlib
open CategoryTheory MonoidalCategory
-- an actegory: a monoidal category C acting on a category D, up to coherent isomorphism
structure Actegory (C : Type*) [Category C] [MonoidalCategory C] (D : Type*) [Category D] where
  act : C × D ⥤ D
  unitor : ∀ X : D, act.obj (𝟙_ C, X) ≅ X
  associator : ∀ (M N : C) (X : D), act.obj (M ⊗ N, X) ≅ act.obj (M, act.obj (N, X))
  -- naturality of `unitor`, `associator` and the pentagon/triangle laws are omitted here
```
tab: Haskell
```haskell
-- The regular actegory of (Hask, (,), ()): M • X = (M, X).
act :: (m -> m') -> (x -> x') -> (m, x) -> (m', x')
act f g (m, x) = (f m, g x)

delta :: ((m, n), x) -> (m, (n, x))      -- (M ⊗ N) • X ≅ M • (N • X)
delta ((m, n), x) = (m, (n, x))

epsilon :: ((), x) -> x                   -- J • X ≅ X
epsilon ((), x) = x
```
````
