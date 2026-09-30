#definition #example

A (finite, time-homogeneous) **Markov chain** on a set of states $S$ is a function $f : S \to \mathrm{Dist}(S)$ assigning to each state a probability distribution over the next state — a Kleisli endomorphism for the [[Distribution Monad]]. Category Theory for Scientists phrases it as a database instance: take the schema $\mathrm{Loop}$ (one object $s$, one arrow $f : s \to s$) and interpret it in the [[Kleisli Category]] of $\mathrm{Dist}$ rather than in $\mathbf{Set}$ — a [[Kleisli Instance]]. A [[Discrete Dynamical System]] is the special case where every distribution is a Kronecker delta.

> Sources: CTfS Example 5.3.4.3, §5.3.4; [[Discrete Dynamical System]] (CTfS Example 3.5.2.9, the deterministic version).

## CTfS Example 5.3.4.3

States $\{1, 2, 3, 4\}$ and transition function

$$
1 \mapsto .5(1) + .5(2), \quad 2 \mapsto 1(2), \quad 3 \mapsto .7(1) + .3(3), \quad 4 \mapsto .4(1) + .3(2) + .3(4),
$$

with the transition matrix (row $x$ is the distribution $f(x)$)

$$
M = \begin{pmatrix} .5 & .5 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ .7 & 0 & .3 & 0 \\ .4 & .3 & 0 & .3 \end{pmatrix}.
$$

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}[column sep=large, row sep=large]
3 \arrow[r, ".7"] \arrow[loop left, ".3"] & 1 \arrow[r, ".5"] \arrow[loop above, ".5"] & 2 \arrow[loop right, "1"] \\
 & 4 \arrow[u, ".4"'] \arrow[ur, ".3"'] \arrow[loop left, ".3"] &
\end{tikzcd}
\end{document}
```

Because Kleisli composition in $\mathrm{Dist}$ is matrix multiplication, the $n$-step transition function $f^n$ (the image of the path $f^n$ in $\mathrm{Loop}$, which presents $\mathbb N$) corresponds to the matrix power $M^n$. Starting in state 4:

| steps | $P(1)$ | $P(2)$ | $P(3)$ | $P(4)$ |
|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 1 |
| 1 | .4 | .3 | 0 | .3 |
| 2 | .32 | .59 | 0 | .09 |
| 3 | .196 | .777 | 0 | .027 |

State 2 is **absorbing** ($2 \mapsto 1(2)$): in the long run all mass flows there, and state 3 is unreachable from 4. So the functor $\mathbb N \to \mathrm{Kl}(\mathrm{Dist})$ says everything about the chain's evolution, and questions like "absorbing states", "stationary distributions" ($p M = p$) and "reachability" are questions about this functor.

## Categorical reading

- A Markov chain is to a [[Discrete Dynamical System]] what a relation is to a function: replace $\mathbf{Set}$ by $\mathrm{Kl}(T)$ for the monad $T$ describing the kind of uncertainty — $\mathrm{Dist}$ for probability, the [[Power Set Monad]] for nondeterminism (then $f^n$ is "reachable in $n$ steps"), the [[Maybe Monad]] for possibly-halting dynamics.
- Several kinds of state and several kinds of transitions (a hidden Markov model: hidden states and emissions) are Kleisli instances on a bigger schema.
- A probabilistic [[Finite State Machine]] is a Kleisli action of a free monoid $\mathrm{List}(\Sigma)$ on $S$ in $\mathrm{Kl}(\mathrm{Dist})$ — a matrix per letter.

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# A Markov chain is a Kleisli arrow S → Dist(S) (CTfS Example 5.3.4.3)
M = [0.5 0.5 0.0 0.0;
     0.0 1.0 0.0 0.0;
     0.7 0.0 0.3 0.0;
     0.4 0.3 0.0 0.3]              # row x = the distribution f(x)
kleisli(M, N) = M * N              # Kleisli composition in Dist = matrix product (weighted sum)
η = [1.0 0 0 0; 0 1 0 0; 0 0 1 0; 0 0 0 1]   # unit: Kronecker deltas
kleisli(η, M) == M == kleisli(M, η)          # unit laws: true
p = [0.0 0.0 0.0 1.0] * M^3                  # where state 4 probably is after 3 steps
round.(p; digits = 3)                        # [0.196 0.777 0.0 0.027]
round.([0.0 0.0 0.0 1.0] * M^100; digits = 3)   # [0.0 1.0 0.0 0.0]: absorbed in state 2
```
tab: Lean
```lean
import Mathlib
-- a Markov chain as a Kleisli arrow for the PMF monad
def step : Fin 4 → PMF (Fin 4) := fun _ => PMF.pure 1   -- (placeholder kernel)
-- the n-step kernel is the n-fold Kleisli composite
def nSteps (f : Fin 4 → PMF (Fin 4)) : ℕ → Fin 4 → PMF (Fin 4)
  | 0 => PMF.pure
  | n + 1 => fun s => (nSteps f n s).bind f
#check @ProbabilityTheory.Kernel     -- measure-theoretic Markov kernels
```
tab: Haskell
```haskell
import Data.List (transpose)

type Matrix = [[Double]]

mmul :: Matrix -> Matrix -> Matrix              -- Kleisli composition in Dist
mmul a b = [ [ sum (zipWith (*) r c) | c <- transpose b ] | r <- a ]

m :: Matrix                                     -- CTfS Example 5.3.4.3
m = [[0.5,0.5,0,0],[0,1,0,0],[0.7,0,0.3,0],[0.4,0.3,0,0.3]]

after :: Int -> [Double] -> [Double]            -- distribution after n steps
after n p = head (iterate (`mmul` m) [p] !! n)
-- after 3 [0,0,0,1] ≈ [0.196, 0.777, 0.0, 0.027]
```
````
