#definition #theorem #example

An **open game** $\mathcal G : (X, S) \to (Y, R)$ (Ghani, Hedges, Winschel & Zahn, Definition 3) is a tuple $(\Sigma_{\mathcal G}, P_{\mathcal G}, C_{\mathcal G}, B_{\mathcal G})$ of

- a set $\Sigma_{\mathcal G}$ of **strategy profiles**;
- a **play** function $P_{\mathcal G} : \Sigma_{\mathcal G} \times X \to Y$ (states in, moves out);
- a **coplay** function $C_{\mathcal G} : \Sigma_{\mathcal G} \times X \times R \to S$ (utilities from the future in, utilities for the past out);
- a **best-response** function $B_{\mathcal G} : X \times (Y \to R) \to \mathrm{Rel}(\Sigma_{\mathcal G})$, giving for each *context* — a state $x$ and a continuation $k : Y \to R$ saying how outcomes will be valued — the relation "$\sigma'$ is a best response to $\sigma$".

Forgetting $B$, the pair $(P, C)$ is a $\Sigma$-[[Parametric Lens|parametrised lens]] $(X, S) \to (Y, R)$: an open game is a parametric lens (strategies = parameters) **plus a best-response relation**. A strategy profile $\sigma$ is a **Nash equilibrium** in context $(x, k)$ if $(\sigma, \sigma) \in B(x, k)$.

> Sources: Ghani, Hedges, Winschel & Zahn, *Compositional Game Theory* [arXiv:1603.04641](https://arxiv.org/abs/1603.04641) ([[Compositional Game Theory|notes]]) Definitions 3–10, Theorem 1; Bolt, Hedges & Zahn, *Bayesian open games* [arXiv:1910.03656](https://arxiv.org/abs/1910.03656) ([[Bayesian Open Games|notes]]); Capucci et al., *Towards Foundations of Categorical Cybernetics* [arXiv:2105.06332](https://arxiv.org/abs/2105.06332) ([[Towards Foundations of Categorical Cybernetics|notes]]) §4 (open games as parametrised optics with a selection functor); Capucci, Ghani, Ledent & Nordvall Forsberg [arXiv:2105.06763](https://arxiv.org/abs/2105.06763) ([[Translating Extensive Form Games to Open Games with Agency|notes]]).

## Atomic games and composition

- A **decision** $D : (X, 1) \to (Y, \mathbb R)$ (Definition 4): strategies are functions $\sigma : X \to Y$, play is $\sigma(x)$, coplay is trivial, and $(\sigma, \sigma') \in B_D(x, k)$ iff $\sigma'(x) \in \arg\max k$. More generally, any **selection function** $\delta : (Y \to R) \to \mathcal P(Y)$ gives a decision (Definition 5).
- **Functions** $f : X \to Y$, $g : R \to S$ lift to strategically trivial games (Definition 7), and **counits** $(X, X) \to (1, 1)$ close a play–coplay loop by handing a player's own outcome back as utility (Definition 8).
- **Sequential** (Definition 9) and **parallel** (Definition 10) composition: play composes forwards, coplay backwards as for lenses, strategy sets multiply, and best responses are defined so that *a profile is an equilibrium of the composite iff each part is a best response in the context the other part creates*.

> **Theorem 1** (after quotienting strategy sets by isomorphism). Open games form a symmetric monoidal category $\mathbf{Game}$, and string diagrams in it describe games built from their parts.

## Three lens-shaped frameworks, three backward passes

| framework | forward | backward pass carries | "equilibrium" | note |
|---|---|---|---|---|
| gradient-based learning | model | a **gradient** | stationary point | [[Gradient-Based Learning with Parametric Lenses]] |
| statistical games | generative model | a **posterior** | free-energy minimum | [[Statistical Game]] |
| open games | play | **coutility** + a **best response** | Nash equilibrium | this note |

All three are $\mathbf{Para}(\mathbf{Optic}(\mathcal C))$ with different $\mathcal C$ and different extra structure (Capucci et al.). A *statistical* game is a one-player degenerate case — "game" there means "a lens with an objective attached", not a strategic interaction. **Bayesian open games** (Bolt, Hedges & Zahn) replace functions by Markov kernels, giving incomplete-information games with Bayesian updating in the backward pass.

## Why machine learning cares

A **GAN** is a two-player zero-sum game: generator and discriminator are two players whose objectives have opposite signs. A single scalar objective descended by every parameter (as in variational inference) cannot express this; an open game can, and its solution concept is Nash rather than stationarity. Actor–critic reinforcement learning is a general-sum (Stackelberg) game of the same shape.

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# A decision (Definition 4) and the Nash condition for a 2×2 simultaneous game.
# Prisoner's dilemma, payoffs (row, column); C = cooperate, D = defect.
payoff = Dict((:C, :C) => (-1, -1), (:C, :D) => (-3, 0), (:D, :C) => (0, -3), (:D, :D) => (-2, -2))
moves = (:C, :D)
best_responses(k) = (m = maximum(k, moves); [y for y in moves if k(y) == m])   # arg max k
is_nash(σ) = σ[1] in best_responses(y -> payoff[(y, σ[2])][1]) &&
             σ[2] in best_responses(y -> payoff[(σ[1], y)][2])
[σ for σ in Iterators.product(moves, moves) if is_nash(σ)]      # [(:D, :D)]
```
tab: Lean
```lean
import Mathlib
-- An open game (X, S) → (Y, R), Ghani–Hedges–Winschel–Zahn Definition 3
structure OpenGame (X S Y R : Type) where
  Strat : Type
  play : Strat × X → Y
  coplay : Strat × X × R → S
  bestResponse : X × (Y → R) → Strat → Strat → Prop

def OpenGame.isNash {X S Y R : Type} (G : OpenGame X S Y R) (x : X) (k : Y → R) (σ : G.Strat) : Prop :=
  G.bestResponse (x, k) σ σ
```
tab: Haskell
```haskell
-- A single decision as an open game, and its best-response check
data Move = C | D deriving (Eq, Show, Enum, Bounded)

payoff :: (Move, Move) -> (Int, Int)
payoff (C, C) = (-1, -1); payoff (C, D) = (-3, 0)
payoff (D, C) = (0, -3);  payoff (D, D) = (-2, -2)

argmaxes :: (Move -> Int) -> [Move]
argmaxes k = let m = maximum (map k [minBound ..]) in [ y | y <- [minBound ..], k y == m ]

isNash :: (Move, Move) -> Bool
isNash (a, b) = a `elem` argmaxes (\y -> fst (payoff (y, b)))
             && b `elem` argmaxes (\y -> snd (payoff (a, y)))
-- filter isNash [ (a, b) | a <- [C, D], b <- [C, D] ] == [(D, D)]
```
````
