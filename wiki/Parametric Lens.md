#definition #example

A **parametric lens** is a morphism of $\mathbf{Para}(\mathbf{Lens}(\mathcal C))$, the [[Para Construction]] applied to the category of [[Lens|lenses]] over a cartesian category $\mathcal C$:

- **objects**: pairs $(A, A')$;
- **morphisms** $(A, A') \to (B, B')$: a parameter pair $(P, P')$ and a lens $(f, f^\sharp) : (A, A') \times (P, P') \to (B, B')$, i.e. two ordinary maps
$$
f : P \times A \to B, \qquad\qquad f^\sharp : P \times A \times B' \to P' \times A'.
$$

Feed it a parameter, an input and a desired change in the output; get back a change in the parameter *and* a change in the input. Three wires in each direction: data left to right, corrections right to left, parameters top-down and parameter updates bottom-up. Composition joins the $(B, B')$ wires and leaves both parameter wires dangling, so the composite has parameter pair $(Q \times P, Q' \times P')$.

> Sources: Cruttwell, Gavranović, Ghani, Wilson & Zanasi [arXiv:2103.01931](https://arxiv.org/abs/2103.01931) ([[Categorical Foundations of Gradient-Based Learning|notes]]) Definition 2.5 and footnote 5 (these are the *learners* of Fong et al.); Fong, Spivak & Tuyéras, *Backprop as Functor* [arXiv:1711.10455](https://arxiv.org/abs/1711.10455) ([[Backprop as Functor - A Compositional Perspective on Supervised Learning|notes]]); Capucci et al. [arXiv:2105.06332](https://arxiv.org/abs/2105.06332) ([[Towards Foundations of Categorical Cybernetics|notes]]) (parametrised optics); Gavranović [arXiv:2403.13001](https://arxiv.org/abs/2403.13001) ([[Fundamental Components of Deep Learning - A Category-Theoretic Approach|notes]]).

```tikz
\usepackage{tikz}
\begin{document}
\begin{tikzpicture}[font=\small]
  \draw[rounded corners] (0,-0.8) rectangle (2.0,0.8);
  \node at (1.0,0) {$f,\ f^\sharp$};
  \draw[->] (-1.6,0.35) -- (0,0.35);   \node at (-1.9,0.35) {$A$};
  \draw[<-] (-1.6,-0.35) -- (0,-0.35); \node at (-1.9,-0.35) {$A'$};
  \draw[->] (2.0,0.35) -- (3.6,0.35);   \node at (3.9,0.35) {$B$};
  \draw[<-] (2.0,-0.35) -- (3.6,-0.35); \node at (3.9,-0.35) {$B'$};
  \draw[->] (0.7,2.0) -- (0.7,0.8);    \node at (0.45,1.6) {$P$};
  \draw[<-] (1.3,2.0) -- (1.3,0.8);    \node at (1.6,1.6) {$P'$};
\end{tikzpicture}
\end{document}
```

## Why $(P, P')$ and not just $P$

The update to a parameter need not live in the same space as the parameter. In $\mathbf{Smooth}$ they coincide ($P' = P = \mathbb R^p$) and this is the case everyone has in mind. But for Boolean circuits over $\mathbb Z_2$ the "gradient" is an XOR mask (Wilson & Zanasi, [arXiv:2101.10488](https://arxiv.org/abs/2101.10488) ([[Reverse Derivative Ascent - A Categorical Approach to Learning Boolean Circuits|notes]])); for a parameter constrained to a manifold — a rotation, a covariance matrix, a point of a Grassmannian — the update lives in a tangent (or cotangent) space; and turning a cotangent into a step needs a metric, which is where natural gradients enter.

## Where parametric lenses come from, and what is still missing

- **From autodiff.** A [[Reverse Derivative Category|reverse derivative]] turns a $\mathbf{Para}(\mathcal C)$-map into a parametric lens: $\mathbf{Para}(R) : \mathbf{Para}(\mathcal C) \to \mathbf{Para}(\mathbf{Lens}(\mathcal C))$. A neural network layer with its `pullback` is exactly such an image.
- **Still missing for learning.** A parametric lens takes a *change* in $B$ and returns a *change* in $P$. When training one has a *target value* $b \in B$ and wants a *new parameter*. The gaps are closed by a loss map, a learning rate and an optimiser — all of them lenses too; see [[Gradient-Based Learning with Parametric Lenses]].
- **Other backward passes.** Replacing lenses by [[Bayesian Lens|Bayesian lenses]] gives the parameterized [[Statistical Game|statistical games]] of AutoBayes (the backward pass is a posterior); replacing them by optics with a selection functor gives [[Open Game|open games]] (the backward pass is a best response).

## Lenticulum.jl

Lux.jl layers are parametric lenses in $\mathbf{Smooth}$; Lenticulum.jl factors are parameterized statistical games, which only *become* a lens once a polarity is chosen. See [Lux as a Parametric Lens](https://mathstruct.org/Lenticulum.jl/dev/vault/Foundations/Lux-as-a-Parametric-Lens).

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# A parametric lens for a dense layer y = tanh.(W x): get(p, x), put(p, x, ȳ) = (p̄, x̄).
struct PLens{G,P}; get::G; put::P; end
dense_tanh = PLens(
    (W, x) -> tanh.(W * x),
    (W, x, ȳ) -> (δ = ȳ .* (1 .- tanh.(W * x) .^ 2); (δ * x', W' * δ)))
# composition: parameters pair up, the backward pass is the reverse chain rule
compose(g::PLens, f::PLens) = PLens(
    ((q, p), x) -> g.get(q, f.get(p, x)),
    ((q, p), x, z̄) -> begin
        y = f.get(p, x)
        q̄, ȳ = g.put(q, y, z̄)
        p̄, x̄ = f.put(p, x, ȳ)
        ((q̄, p̄), x̄)
    end)
net = compose(dense_tanh, dense_tanh)
W1, W2, x = [0.5 -0.2; 0.1 0.3], [1.0 0.4], [1.0, 2.0]
(ḡ2, ḡ1), x̄ = net.put((W2, W1), x, [1.0])
# check the first-layer gradient against finite differences
L(W1) = sum(tanh.(W2 * tanh.(W1 * x)))
e = [1.0 0; 0 0]; h = 1e-6
isapprox(ḡ1[1, 1], (L(W1 + h * e) - L(W1 - h * e)) / 2h; atol = 1e-6)   # true
```
tab: Lean
```lean
import Mathlib
-- A parametric lens between (A, A') and (B, B') with parameter pair (P, P').
structure PLens (P P' A A' B B' : Type) where
  get : P × A → B
  put : P × A × B' → P' × A'

def PLens.comp {P P' Q Q' A A' B B' C C' : Type}
    (f : PLens P P' A A' B B') (g : PLens Q Q' B B' C C') : PLens (Q × P) (Q' × P') A A' C C' where
  get := fun ((q, p), a) => g.get (q, f.get (p, a))
  put := fun ((q, p), a, c') =>
    let (q', b') := g.put (q, f.get (p, a), c')
    let (p', a') := f.put (p, a, b')
    ((q', p'), a')
```
tab: Haskell
```haskell
-- A parametric lens: get :: (p, a) -> b ; put :: (p, a, b') -> (p', a')
data PLens p p' a a' b b' = PLens { get :: (p, a) -> b, put :: (p, a, b') -> (p', a') }

(|>) :: PLens p p' a a' b b' -> PLens q q' b b' c c' -> PLens (q, p) (q', p') a a' c c'
PLens f f' |> PLens g g' = PLens
  (\((q, p), a) -> g (q, f (p, a)))
  (\((q, p), a, c') -> let (q', b') = g' (q, f (p, a), c')
                           (p', a') = f' (p, a, b')
                       in ((q', p'), a'))

-- a scalar linear layer y = w x
linear :: PLens Double Double Double Double Double Double
linear = PLens (\(w, x) -> w * x) (\(w, x, dy) -> (x * dy, w * dy))
-- put (linear |> linear) ((2, 3), 5, 1) == ((15, 10), 6)
```
````
