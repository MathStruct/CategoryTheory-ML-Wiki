#definition #example #program

Cruttwell, Gavranović, Ghani, Wilson & Zanasi show that the four things one names when setting up a training run — **model, loss, learning rate, optimiser** — are all [[Parametric Lens|parametric lenses]], and that "setting up training" is **composition** in $\mathbf{Para}(\mathbf{Lens}(\mathcal C))$ for a [[Reverse Derivative Category|reverse derivative category]] $\mathcal C$:

$$
\underbrace{\text{optimiser}}_{\text{reparametrisation}} \;\text{ on top of }\; \underbrace{\text{model}}_{\mathbf{Para}(R)(f)} \mathbin{;} \underbrace{\text{loss}}_{\text{parameter = label}} \mathbin{;} \underbrace{\text{learning rate}}_{\text{cap}} .
$$

After composing, the only dangling wires are the ones the user supplies: the input $A$, the label $B$ (on the loss's parameter wire) and the optimiser state. One training step is: run the composite lens's `get`, then its `put`.

> Sources: Cruttwell et al., *Categorical Foundations of Gradient-Based Learning* [arXiv:2103.01931](https://arxiv.org/abs/2103.01931) ([[Categorical Foundations of Gradient-Based Learning|notes]]) §3 (Definitions 3.3, 3.8, 3.11, 3.14; Examples 3.1–3.18), §4 (Examples 4.1–4.5: supervised learning, Boolean circuits, deep dreaming); Wilson & Zanasi [arXiv:2101.10488](https://arxiv.org/abs/2101.10488) ([[Reverse Derivative Ascent - A Categorical Approach to Learning Boolean Circuits|notes]]); Fong, Spivak & Tuyéras [arXiv:1711.10455](https://arxiv.org/abs/1711.10455) ([[Backprop as Functor]]).

## Model (Examples 3.1–3.2)

A $\mathbf{Para}(\mathcal C)$-map $(P, f) : A \to B$ pushed through $\mathbf{Para}(R)$ gives the parametric lens $(f, R[f])$. In a deep-learning library: a layer plus its `pullback`.

## Loss map (Definition 3.3)

> A **loss map** on $B$ is a $\mathbf{Para}(\mathcal C)$-map $(\mathrm{loss}, B) : B \to L$.

**The loss map's parameter is the label.** A loss $B \times B \to L$ with one argument the prediction and the other the ground truth is naturally a $B$-parametrised map $B \to L$. Putting labels and weights on the same footing is what later lets the loss itself be learned (GANs) and lets data be just another node in a graph.

| loss (Examples 3.4–3.7) | $L$ | $e(b_t, b_p)$ | $R[e](b_t, b_p, \alpha)$ |
|---|---|---|---|
| quadratic | $\mathbb R$ | $\tfrac12\sum_i ((b_p)_i - (b_t)_i)^2$ | $\alpha \cdot (b_p - b_t,\ b_t - b_p)$ |
| Boolean | $\mathbb Z_2^b$ | $b_t + b_p$ (XOR) | $(\alpha, \alpha)$ |
| softmax cross-entropy | $\mathbb R$ | $\sum_i (b_t)_i\bigl((b_p)_i - \log \mathrm{softmax}(b_p)_i\bigr)$ | (see paper) |
| dot product | $\mathbb R$ | $b_t \cdot b_p$ | $\alpha\cdot(b_p,\ b_t)$ |

## Learning rate (Definition 3.8)

> A **learning rate** $\alpha$ on $L$ is a lens $(L, L') \to (1, 1)$.

Its get is forced (the unique map to $1$), so all content is in the put $\alpha^\sharp : L \to L'$, which **caps off** the dangling loss wire. In $\mathbf{Smooth}$, $\alpha^\sharp(l) = -\epsilon$, a constant (Example 3.9) — the minus sign is where *descent* enters, and only there. Because the cap discards $l$, **the numerical loss value is never used by gradient descent**; only its derivative is. In $\mathbf{Poly}_{\mathbb Z_2}$ the learning rate is the identity (Example 3.10).

## Optimiser as reparametrisation (Definitions 3.11, 3.14)

After capping, the composite takes a parameter and returns a parameter *update*. An optimiser is a box on the $(P, P')$ wires — a lens $(P, P) \to (P, P')$ or, with state $S$, $(S \times P, S \times P) \to (P, P')$ — i.e. a **reparametrisation** ([[Para Construction]]).

| optimiser | $S$ | get $U(s, p)$ | put $U^\sharp(s, p, p')$ |
|---|---|---|---|
| gradient update (Def. 3.11) | $1$ | $p$ | $p + p'$ |
| momentum (Ex. 3.15) | $P$ | $p$ | $(s',\ p + s')$, $s' = -\gamma s + p'$ |
| Nesterov (Ex. 3.16) | $P$ | $p + \gamma s$ | $(s',\ p + s')$, $s' = -\gamma s + p'$ |
| Adagrad (Ex. 3.17) | $P$ | $p$ | $(g',\ p + \frac{\epsilon}{\delta + \sqrt{g'}} \odot p')$, $g' = g + p' \odot p'$ |

**Nesterov** is the example that justifies the machinery: its get is *not the identity* ($p + \gamma s$). "Evaluate the gradient at the look-ahead point" is literally "the forward part of the optimiser lens is non-trivial" — no other formalism makes that as visible. Adam (Example 3.18) fits the same shape with two state components.

```tikz
\usepackage{tikz}
\begin{document}
\begin{tikzpicture}[font=\small]
  \draw[rounded corners] (0,-0.8) rectangle (1.9,0.8);
  \node at (0.95,0) {model};
  \draw[->] (-1.5,0.35) -- (0,0.35);   \node at (-1.8,0.35) {$A$};
  \draw[<-] (-1.5,-0.35) -- (0,-0.35); \node at (-1.8,-0.35) {$A'$};
  \draw[rounded corners] (3.1,-0.8) rectangle (5.0,0.8);
  \node at (4.05,0) {loss};
  \draw[->] (1.9,0.35) -- (3.1,0.35);   \node at (2.5,0.65) {$B$};
  \draw[<-] (1.9,-0.35) -- (3.1,-0.35); \node at (2.5,-0.65) {$B'$};
  \draw[->] (3.75,2.0) -- (3.75,0.8);   \node at (3.5,1.6) {$B$};
  \draw[<-] (4.35,2.0) -- (4.35,0.8);   \node at (4.6,1.6) {$B'$};
  \draw[rounded corners] (6.2,-0.8) rectangle (7.3,0.8);
  \node at (6.75,0) {$\alpha$};
  \draw[->] (5.0,0.35) -- (6.2,0.35);   \node at (5.6,0.65) {$L$};
  \draw[<-] (5.0,-0.35) -- (6.2,-0.35); \node at (5.6,-0.65) {$L'$};
  \draw[rounded corners] (0,2.0) rectangle (1.9,3.4);
  \node at (0.95,2.7) {optimiser};
  \draw[->] (0.65,2.0) -- (0.65,0.8);   \node at (0.35,1.4) {$P$};
  \draw[<-] (1.25,2.0) -- (1.25,0.8);   \node at (1.6,1.4) {$P'$};
  \draw[->] (0.65,4.4) -- (0.65,3.4);   \node at (0.1,4.0) {$S{\times}P$};
  \draw[<-] (1.25,4.4) -- (1.25,3.4);   \node at (1.9,4.0) {$S{\times}P$};
\end{tikzpicture}
\end{document}
```

## Deep dreaming: the same lens, a different open wire (§4.2)

Keep the parameters fixed and leave the **input** wire open instead: the backward pass now updates the *input* to increase a class score (dot-product loss, Example 4.5). Supervised learning and deep dreaming are the same composite with different wires treated as data — a first glimpse of the direction-agnostic view that relational and Bayesian frameworks take further.

## Lenticulum.jl

In Lenticulum.jl data, losses and optimisers become ordinary nodes of a factor graph: a loss is a sink factor (the learning-rate cap), an optimiser is a bidirectional factor on an exposed parameter variable, and the Nesterov lens is checked in its test suite. See [Everything is a Factor](https://mathstruct.org/Lenticulum.jl/dev/vault/Factor-Graphs/Everything-is-a-Factor).

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# One gradient step as a composite of lenses: model ; loss ; learning-rate cap, with an optimiser.
model(p, x) = p[1] * x + p[2]                         # a Para(Smooth) map with P = ℝ²
R_model(p, x, ȳ) = ([x * ȳ, ȳ], p[1] * ȳ)             # its reverse derivative: (p̄, x̄)
loss(bt, bp) = (bp - bt)^2 / 2                        # label bt is the loss's parameter
R_loss(bt, bp, l̄) = (l̄ * (bt - bp), l̄ * (bp - bt))   # (b̄t, b̄p), Example 3.4
cap(l; ϵ = 0.1) = -ϵ                                  # learning-rate lens (L,L′) → (1,1)
function step(p, x, bt)
    bp = model(p, x); l = loss(bt, bp)                # get
    _, b̄p = R_loss(bt, bp, cap(l))                    # put, from the cap backwards
    p̄, _ = R_model(p, x, b̄p)
    p .+ p̄                                            # gradient-update reparametrisation p + p′
end
data = [(x, 3x - 1) for x in -2:0.5:2]
let p = [0.0, 0.0]
    for epoch in 1:200, (x, y) in data; p = step(p, x, y); end
    round.(p; digits = 3)                             # ≈ [3.0, -1.0]
end
# Nesterov: the optimiser lens has a non-trivial get p + γ s
nesterov_get(s, p; γ = 0.9) = p .+ γ .* s
nesterov_put(s, p, p′; γ = 0.9) = (s′ = -γ .* s .+ p′; (s′, p .+ s′))
```
tab: Lean
```lean
import Mathlib
-- The four learning components as lens data over ℝ (get / put pairs).
structure Lens' (A A' B B' : Type) where
  get : A → B
  put : A × B' → A'

-- a learning rate is a lens (L, L') → (Unit, Unit): all content is in the put
def learningRate (ε : ℝ) : Lens' ℝ ℝ Unit Unit where
  get := fun _ => ()
  put := fun _ => -ε

-- gradient update as a reparametrisation lens (P, P) → (P, P')
def gradientUpdate : Lens' ℝ ℝ ℝ ℝ where
  get := id
  put := fun (p, p') => p + p'
```
tab: Haskell
```haskell
-- Supervised learning of y = w x + b by composing model, loss and learning-rate lenses.
type P = (Double, Double)

model :: P -> Double -> Double
model (w, b) x = w * x + b
rModel :: P -> Double -> Double -> (P, Double)          -- reverse derivative (p̄, x̄)
rModel (w, _) x dy = ((x * dy, dy), w * dy)

rLoss :: Double -> Double -> Double -> Double           -- ∂/∂bp of (bp - bt)²/2, times l̄
rLoss bt bp dl = dl * (bp - bt)

step :: Double -> P -> (Double, Double) -> P
step eps p (x, bt) =
  let bp = model p x
      dbp = rLoss bt bp (negate eps)                     -- the cap supplies l̄ = -ε
      ((dw, db), _) = rModel p x dbp
  in (fst p + dw, snd p + db)

train :: P
train = foldl (step 0.1) (0, 0) (concat (replicate 200 [ (x, 3 * x - 1) | x <- [-2, -1.5 .. 2] ]))
-- train ≈ (3.0, -1.0)
```
````
