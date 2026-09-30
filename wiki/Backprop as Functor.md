#theorem #definition #example

Fong, Spivak and Tuyéras define a **learner** $A \to B$ as a tuple $(P, I, U, r)$ of a parameter set $P$ and functions

$$
I : P \times A \to B \ (\text{implement}), \qquad U : P \times A \times B \to P \ (\text{update}), \qquad r : P \times A \times B \to A \ (\text{request}),
$$

taken up to relabelling of parameters. Learners compose — the request $r$ of the second learner is the *training target* passed back to the first — and form a symmetric monoidal category $\mathbf{Learn}$ (Proposition II.4). The main theorem:

> **Theorem III.2.** Fix a step size $\varepsilon > 0$ and a differentiable error function $e(x, y)$ with $\partial e / \partial x (x_0, -)$ invertible for each $x_0$. Then gradient descent and backpropagation define a faithful, injective-on-objects, strong symmetric monoidal functor
> $$L_{\varepsilon, e} : \mathbf{Para} \longrightarrow \mathbf{Learn}$$
> sending a parametrised function $I : P \times A \to B$ to the learner with $U_I(p, a, b) = p - \varepsilon \nabla_p E_I(p, a, b)$ and $r_I(p, a, b) = f_a\bigl(\nabla_a E_I(p, a, b)\bigr)$, where $E_I = \sum_j e(I_j(p, a), b_j)$.

**Backpropagation is functorial**: training a composite network by gradient descent is the same as composing the learners obtained from its layers. The request map $r$ is the new ingredient — it is what a layer asks its predecessor to output instead.

> Sources: Fong, Spivak & Tuyéras, *Backprop as Functor: A compositional perspective on supervised learning* [arXiv:1711.10455](https://arxiv.org/abs/1711.10455) ([[Backprop as Functor - A Compositional Perspective on Supervised Learning|notes]]) Definitions II.1, III.1, Proposition II.4, Theorem III.2; Fong & Johnson, *Lenses and Learners* [arXiv:1903.03671](https://arxiv.org/abs/1903.03671) ([[Lenses and Learners|notes]]); Cruttwell et al. [arXiv:2103.01931](https://arxiv.org/abs/2103.01931) ([[Categorical Foundations of Gradient-Based Learning|notes]]) footnote 5, §6.

## Learners are parametric lenses

Pair the update and request into one map $(U, r) : P \times A \times B \to P \times A$ and the learner is a lens $(P \times A, P \times A) \to (B, B)$ with a parameter — a [[Parametric Lens]], with the backward direction carrying *targets* rather than gradients. Fong & Johnson make this precise: learners embed in a category of (asymmetric) lenses, and the lens laws correspond to well-behaved learning. Cruttwell et al. (footnote 5) note that their parametric lenses *are* these learners, generalised from $\mathbf{Set}$ to any [[Reverse Derivative Category]] and from a fixed loss and step size to arbitrary loss maps, learning rates and optimisers ([[Gradient-Based Learning with Parametric Lenses]]).

## What the theorem buys

- **Modularity**: a neural network's learner is determined by its layers' learners; the global learning rule need never be written down.
- **Wiring diagrams**: because $L_{\varepsilon,e}$ is *monoidal*, the string diagram of a network — layers in series and in parallel, with copying and summing of wires — can be read as a diagram in $\mathbf{Learn}$.
- **A caveat recorded in the paper**: the invertibility condition on $\partial e/\partial x$ holds for quadratic error but not for every loss; later formulations (Cruttwell et al.) avoid it by separating the loss map from the learning rate.

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# A learner (I, U, r) and its composite; check that training a composite equals
# composing the learners of the parts (Theorem III.2 in miniature, quadratic error).
struct Learner{I,U,R}; I::I; U::U; r::R; end
const ε = 0.05
function gd_learner(I, ∇p, ∇a)          # ∇p, ∇a of E(p,a,b) = ½(I(p,a) - b)²
    Learner(I, (p, a, b) -> p - ε * ∇p(p, a, b), (p, a, b) -> a - ∇a(p, a, b))
end
lin = gd_learner((p, a) -> p * a, (p, a, b) -> (p * a - b) * a, (p, a, b) -> (p * a - b) * p)
function compose(L2::Learner, L1::Learner)
    Learner(((q, p), a) -> L2.I(q, L1.I(p, a)),
            ((q, p), a, c) -> (b = L1.I(p, a); (L2.U(q, b, c), L1.U(p, a, L2.r(q, b, c)))),
            ((q, p), a, c) -> L1.r(p, a, L2.r(q, L1.I(p, a), c)))
end
net = compose(lin, lin)
let θ = (1.0, 0.5)
    for _ in 1:2000, a in (-1.0, 0.5, 2.0); θ = net.U(θ, a, 6a); end   # learn a ↦ 6a
    round(net.I(θ, 1.0); digits = 2)                                    # ≈ 6.0
end
```
tab: Lean
```lean
import Mathlib
-- Fong–Spivak–Tuyéras learners (Definition II.1)
structure Learner (A B : Type) where
  P : Type
  implement : P × A → B
  update : P × A × B → P
  request : P × A × B → A

def Learner.comp {A B C : Type} (L₁ : Learner A B) (L₂ : Learner B C) : Learner A C where
  P := L₂.P × L₁.P
  implement := fun ((q, p), a) => L₂.implement (q, L₁.implement (p, a))
  update := fun ((q, p), a, c) =>
    let b := L₁.implement (p, a)
    (L₂.update (q, b, c), L₁.update (p, a, L₂.request (q, b, c)))
  request := fun ((q, p), a, c) => L₁.request (p, a, L₂.request (q, L₁.implement (p, a), c))
```
tab: Haskell
```haskell
{-# LANGUAGE ExistentialQuantification #-}
-- A learner a -> b with a hidden parameter type p (Definition II.1)
data Learner a b = forall p. Learner p (p -> a -> b) (p -> a -> b -> p) (p -> a -> b -> a)

compose :: Learner b c -> Learner a b -> Learner a c
compose (Learner q i2 u2 r2) (Learner p i1 u1 r1) =
  Learner (q, p)
          (\(q', p') a -> i2 q' (i1 p' a))
          (\(q', p') a c -> let b = i1 p' a in (u2 q' b c, u1 p' a (r2 q' b c)))
          (\(q', p') a c -> r1 p' a (r2 q' (i1 p' a) c))
```
````
