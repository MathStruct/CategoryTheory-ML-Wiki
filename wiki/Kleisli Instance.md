#definition #example

Let $\mathcal C$ be a [[Database Schema|schema]] and $(T, \eta, \mu)$ a [[Monad]] on $\mathbf{Set}$. A **Kleisli instance** of $\mathcal C$ for $T$ is a [[Functor]]

$$
I : \mathcal C \to \mathrm{Kl}(T)
$$

into the [[Kleisli Category]] of $T$. It assigns a set $I(c)$ to each object as usual, but each arrow $f : c \to c'$ of the schema becomes a *$T$-valued* function $I(c) \to T(I(c'))$ — a foreign key whose value may be missing, multiple, random, … — and the path equivalences must hold for Kleisli composition. Ordinary instances ([[C-Set|$\mathcal C$-sets]]) are the Kleisli instances that factor through $\mathbf{Set} \to \mathrm{Kl}(T)$, $f \mapsto \eta \circ f$. "Monads formalize context", and Kleisli instances put that context into a database (CTfS §5.3).

> Sources: CTfS §5.3.4 (Example 5.3.4.1, 5.3.4.2, Example 5.3.4.3, Application 5.3.4.4), Remark 5.3.2.7.

## Examples

- **Maybe: graphs with dangling edges** (CTfS Example 5.3.4.1). On the graph schema $E \rightrightarrows V$ with the [[Maybe Monad]] $X \mapsto X \sqcup \{\star\}$, $\mathrm{src}$ and $\mathrm{tgt}$ become *partial* functions. An instance with vertices $v, w, x$ and arrows $f : v \to w$, $g, h : w \to x$, $i : v \to \star$ and $j : \star \to \star$ has an arrow $i$ with no target and an arrow $j$ with neither endpoint — the kind of incomplete data real databases contain (a `NULL` foreign key).
- **Dist: Markov chains** (CTfS Example 5.3.4.3). On the schema $\mathrm{Loop}$ with the [[Distribution Monad]], an instance is a set of states with a random next-state function — a [[Markov Chain]]. On a longer schema, a chain $W \to E$ of stochastic maps as in the star-and-detector example.
- **Power set: relations.** With the [[Power Set Monad]], $\mathrm{Kl}(\mathcal P) = \mathbf{Rel}$, so every foreign key becomes a relation: on the graph schema, an arrow may have several sources and targets, a kind of hypergraph. On $\mathrm{Loop}$, it is a nondeterministic dynamical system.
- **Paths: schema morphisms.** CTfS Remark 5.3.2.7: the $\mathrm{Paths}$ monad on $\mathbf{Grph}$ has Kleisli arrows $G \to \mathrm{Paths}(G')$ — graph maps sending arrows to paths — which is how schema morphisms are defined ([[Categories and Schemas are Equivalent]]).

## Composing context

A path in the schema is interpreted by Kleisli composition, so the monad dictates how context propagates along foreign-key chains: for Maybe, a missing value anywhere makes the whole path missing (`NULL` propagation); for $\mathrm{Dist}$, probabilities along the path multiply and alternatives add; for $\mathcal P$, all reachable values are collected. Path equivalences are then *equations between Kleisli composites*, which may hold or fail depending on the monad.

````tabs
tab: Julia
**Docs:** [ACSets API](https://algebraicjulia.github.io/ACSets.jl/stable/api/) · [Theories & presentations](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/theories/)
```julia
using Catlab
# A Kleisli instance for the Maybe monad on the graph schema: arrows may lack endpoints
# (CTfS Example 5.3.4.1). Encode "no value" as `nothing` in a Julia attribute.
@present SchPartialGraph(FreeSchema) begin
  (E, V)::Ob
  Endpoint::AttrType
  (src, tgt)::Attr(E, Endpoint)
end
@acset_type PartialGraph(SchPartialGraph)
J = @acset PartialGraph{Union{Int,Nothing}} begin
  V = 3; E = 5
  src = [1, 2, 2, 1, nothing]; tgt = [2, 3, 3, nothing, nothing]   # f, g, h, i (dangling), j
end
count(isnothing, J[:, :tgt])     # 2 arrows with no target
# Kleisli composition for Maybe: a missing value anywhere makes the path missing
kcomp(f, g) = x -> (y = f(x); isnothing(y) ? nothing : g(y))
out_edge(v) = get(Dict(1 => 1, 2 => 2), v, nothing)        # partial V → E: v ↦ f, w ↦ g
step = kcomp(out_edge, e -> J[e, :tgt])                    # follow the out-edge to its target
step(1), kcomp(step, step)(1), kcomp(step, step)(2)        # (2, 3, nothing): x has no out-edge
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
-- the Kleisli category of a monad on Type, and functors into it
#check @KleisliCat                        -- KleisliCat m for a Monad m
-- a Maybe-valued (Option) edge endpoint: a partial foreign key
structure PartialGraph where
  V : Type
  E : Type
  src : E → Option V
  tgt : E → Option V
```
tab: Haskell
```haskell
import Control.Monad ((>=>))

-- a Kleisli instance of the graph schema for Maybe: partial foreign keys
data V = Vv | Vw | Vx deriving (Show, Eq)
data E = F | G | H | I | J deriving (Show, Eq)

src, tgt :: E -> Maybe V
src F = Just Vv; src G = Just Vw; src H = Just Vw; src I = Just Vv; src J = Nothing
tgt F = Just Vw; tgt G = Just Vx; tgt H = Just Vx; tgt I = Nothing; tgt J = Nothing

-- paths are interpreted by Kleisli composition (>=>): Nothing propagates like NULL
firstOut :: V -> Maybe E
firstOut Vv = Just F; firstOut Vw = Just G; firstOut Vx = Nothing
twoSteps :: V -> Maybe V
twoSteps = firstOut >=> tgt >=> firstOut >=> tgt   -- twoSteps Vv == Just Vx
```
````
