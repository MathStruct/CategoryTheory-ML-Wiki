#definition #theorem #example #program

A **change action** $\hat A = (A, \Delta A, \oplus_A)$ is a set $A$ of values, a [[Monoid]] $(\Delta A, \cdot, 0)$ of **changes**, and a [[Monoid Action]] $\oplus_A : A \times \Delta A \to A$ — applying the empty change does nothing, and applying two changes in turn is applying their product. A **derivative** of a function $f : A \to B$ between change actions is a function $f' : A \times \Delta A \to \Delta B$ with

$$
f(a \oplus_A \delta a) \;=\; f(a) \oplus_B f'(a, \delta a)
$$

**exactly**, not up to a first-order error: $f'$ says how to update the output when the input changes. Derivatives compose by the **chain rule** $(g \circ f)'(a, \delta a) = g'(f(a), f'(a, \delta a))$. This is the mathematics of **incremental computation**: if $f$ is expensive and $f'$ cheap, keep $f(a)$ and update it.

> Sources: Alvarez-Picallo, Eyers-Taylor, Peyton Jones & Ong, *Fixing Incremental Computation: Derivatives of Fixpoints, and the Recursive Semantics of Datalog*, ESOP 2019, [arXiv:1811.06069](https://arxiv.org/abs/1811.06069) ([[Fixing Incremental Computation - Derivatives of Fixpoints, and the Recursive Semantics of Datalog|notes]]) Definitions 1, 2, 4, 5, 40, Theorems 3, 27, 39, 43, Proposition 6; Alvarez-Picallo & Ong, *Change Actions: Models of Generalised Differentiation*, FoSSaCS 2019, [arXiv:1902.05465](https://arxiv.org/abs/1902.05465) ([[Change Actions - Models of Generalised Differentiation|notes]]) Definitions 2.2, 2.5, 2.9, 2.13, 4.1, 4.2, Theorems 3.1, 3.2, 4.8, 5.1, Remark 2.1; Cai, Giarrusso, Rendel & Ostermann, *A Theory of Changes for Higher-Order Languages*, PLDI 2014, [arXiv:1312.0658](https://arxiv.org/abs/1312.0658) ([[A Theory of Changes for Higher-Order Languages|notes]]) Definition 2.1, Theorems 2.7, 3.11.

## Examples

| values $A$ | changes $\Delta A$ | action $\oplus$ | a derivative |
|---|---|---|---|
| $\mathbb Z$ | $(\mathbb Z, +, 0)$ | $+$ | $f(x) = x^2$: $f'(x, \delta) = 2x\delta + \delta^2$ |
| sets $\mathcal P(X)$ | $(\mathcal P(X), \cup, \emptyset)$ | $\cup$ | a join $P \mapsto P \bowtie E$: $\delta \mapsto \delta \bowtie E$ |
| sets $\mathcal P(X)$ | pairs (insertions, deletions) | $(P \setminus D) \cup I$ | Datalog with negation (Theorem 27) |
| any set $A$ | $A$ with "replace" | $a \oplus b = b$ | every $f$ has the trivial derivative $f(a \oplus \delta)$ |
| smooth maps | tangent vectors | $+$ in a vector space | the differential — only *approximately* |

The last row is the reason for the name: change actions generalise the derivative of calculus by dropping linearity and asking for exactness. A change action is **complete** if every two values are connected by some change (Definition 4); then a "minus" $b \ominus a$ exists and every function is differentiable (Proposition 6), though perhaps only trivially.

## Semi-naïve evaluation is a derivative

The immediate-consequence step of the transitive-closure program, $T(P) = E \cup (P \bowtie E)$, has derivative $T'(P, \delta) = \delta \bowtie E$ for insertion-only changes, because joins distribute over unions. Iterating a monotone map from $\bot$ while feeding each round only the *change* produced by the last round computes the same [[Least Fixed Point]]:

> **Theorem 39 (incremental computation of least fixed points).** For a complete, continuous change action and a continuous, differentiable $f$, the least fixed point of $f$ is the limit of the sequence obtained by iterating $(x, \delta) \mapsto (x \oplus \delta,\; f'(x, \delta))$ from $(\bot, \bot)$.

That is semi-naïve evaluation, proved once for every differentiable program rather than once per language feature. Theorem 43 goes further and differentiates the fixed-point operator *itself*, so that when the base facts change one can update an already computed fixed point instead of recomputing it — incremental view maintenance for recursive queries.

## The categorical structure

Change actions and differential maps (a function together with a *regular* derivative, Definitions 2.5, 2.9) form a category $\mathbf{CAct}$; it has products (Theorem 3.1) and a terminal object (Theorem 3.2), and the construction can be internalised in any cartesian category $\mathcal C$ as $\mathbf{CAct}(\mathcal C)$. A **change action model** is a coalgebra $\alpha : \mathcal C \to \mathbf{CAct}(\mathcal C)$ of the copointed endofunctor $\mathbf{CAct}$ (Definition 4.1): a uniform choice of change action on every object and derivative for every morphism. Every model has a **tangent bundle functor** $T A = A \times \Delta A$, $T f = \langle f \circ \pi_1, f' \rangle$ (Definition 4.2) — the same shape as in [[Cartesian Differential Category|cartesian differential categories]], which are one source of examples (Theorem 5.1). Polynomials over a commutative Kleene algebra are another, whose derivatives are *not* additive in the change: change actions are strictly more general.

Change *structures* (Cai et al., Definition 2.1) are the earlier, dependently typed version, with a built-in $\ominus$; they give a static program transformation $\mathrm{Derive}$ on the simply typed λ-calculus whose correctness is Theorem 3.11. Function spaces carry change structures (Theorem 2.7), so higher-order programs differentiate too.

## Sophia

Sophia makes incremental compilation fall out of content addressing: a changed definition gets a new hash, and every memoised query result keyed on the old hash is simply not found. That gives correctness but not efficiency — recomputing a fixed point (transitive dependencies, equivalence closure, a saturation run) after a small edit still starts from scratch. Change actions are the theory of doing better: a derivative of each stored relation with respect to inserted edges, and Theorem 43 for the recursive ones. See [Compilation as Query](https://mathstruct.org/Sophia/vault/Design/Compilation-as-Query) and [State of the Art - Incremental Computation](https://mathstruct.org/Sophia/vault/State-of-the-Art/State-of-the-Art---Incremental-Computation).

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# A change action (A, ΔA, ⊕): a monoid ΔA of changes acting on a set A of values.
# Integers with additive changes: (ℤ, (ℤ, +, 0), +).  A derivative f′ satisfies f(a ⊕ δ) = f(a) ⊕ f′(a, δ).
f(x) = x^2;  df(x, δ) = 2x * δ + δ^2          # exact, not a linear approximation
g(y) = 3y + 1; dg(y, δ) = 3δ
all(f(a + δ) == f(a) + df(a, δ) for a in -5:5, δ in -5:5)                       # true
# chain rule (Theorem 3): (g ∘ f)′(a, δ) = g′(f(a), f′(a, δ))
all(g(f(a + δ)) == g(f(a)) + dg(f(a), df(a, δ)) for a in -5:5, δ in -5:5)      # true
# Sets with insertions as changes: (𝒫(X), (𝒫(X), ∪, ∅), ∪).  The Datalog step T(P) = E ∪ (P ; E):
E = Set([(1, 2), (2, 3), (3, 1)])
join(P) = Set((x, z) for (x, y) in P for (y2, z) in E if y == y2)
T(P) = E ∪ join(P)
dT(P, δ) = join(δ)                           # the semi-naive delta: only the new facts are joined
P = Set([(1, 2)]); δ = Set([(2, 3), (3, 3)])
T(P ∪ δ) == T(P) ∪ dT(P, δ)                  # true: dT is a derivative of T, because join distributes over ∪
# iterating with the derivative reaches the same least fixed point as naive iteration (Theorem 39)
function lfp_naive(T); P = Set{Tuple{Int,Int}}(); while (Q = T(P)) != P; P = Q; end; P; end
function lfp_incremental(T, dT)
    P = Set{Tuple{Int,Int}}(); Δ = T(P)
    while !isempty(Δ); new = setdiff(Δ, P); union!(P, new); Δ = setdiff(dT(P, new), P); end
    P
end
lfp_naive(T) == lfp_incremental(T, dT)       # true
length(lfp_naive(T))                         # 9: on a 3-cycle every vertex reaches every vertex
```
tab: Lean
```lean
import Mathlib
-- A change action is a monoid of changes acting on values: Mathlib's `AddAction ΔA A` (δ +ᵥ a).
-- a derivative of f : A → B is f' with f (δ +ᵥ a) = f' a δ +ᵥ f a
def IsDerivative {A B ΔA ΔB : Type*} [AddMonoid ΔA] [AddMonoid ΔB] [AddAction ΔA A] [AddAction ΔB B]
    (f : A → B) (f' : A → ΔA → ΔB) : Prop :=
  ∀ a δ, f (δ +ᵥ a) = f' a δ +ᵥ f a

-- the chain rule (Alvarez-Picallo et al., Theorem 3)
theorem chain {A B C ΔA ΔB ΔC : Type*} [AddMonoid ΔA] [AddMonoid ΔB] [AddMonoid ΔC]
    [AddAction ΔA A] [AddAction ΔB B] [AddAction ΔC C]
    (f : A → B) (g : B → C) (f' : A → ΔA → ΔB) (g' : B → ΔB → ΔC)
    (hf : IsDerivative f f') (hg : IsDerivative g g') :
    IsDerivative (g ∘ f) (fun a δ => g' (f a) (f' a δ)) := by
  intro a δ
  simp only [Function.comp, hf a δ, hg (f a) (f' a δ)]
```
tab: Haskell
```haskell
-- Exact derivatives for the change action (Integer, (+)) and the chain rule
f, g :: Integer -> Integer
f x = x * x
g y = 3 * y + 1

df, dg :: Integer -> Integer -> Integer
df x d = 2 * x * d + d * d
dg _ d = 3 * d

main :: IO ()
main = print ( and [ f (a + d) == f a + df a d | a <- [-5 .. 5], d <- [-5 .. 5] ]
             , and [ g (f (a + d)) == g (f a) + dg (f a) (df a d) | a <- [-5 .. 5], d <- [-5 .. 5] ] )
-- (True,True)
```
````
