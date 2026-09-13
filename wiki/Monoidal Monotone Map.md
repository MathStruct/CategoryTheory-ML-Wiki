#definition #example

Let $\mathcal{P} = (P, \leq_P, I_P, \otimes_P)$ and $\mathcal{Q} = (Q, \leq_Q, I_Q, \otimes_Q)$ be [[Symmetric Monoidal Preorder|monoidal preorders]]. A **monoidal monotone** from $\mathcal{P}$ to $\mathcal{Q}$ is a [[Monotone Map]] $f : (P, \leq_P) \to (Q, \leq_Q)$ such that

(a) $I_Q \leq_Q f(I_P)$, and
(b) $f(p_1) \otimes_Q f(p_2) \leq_Q f(p_1 \otimes_P p_2)$ for all $p_1, p_2 \in P$.

It is **strong** if (a′) $I_Q \cong f(I_P)$ and (b′) $f(p_1) \otimes_Q f(p_2) \cong f(p_1 \otimes_P p_2)$, and **strict** if these hold with $=$. Monoidal monotones are the preorder case of (lax) [[Monoidal Functor|monoidal functors]] (7 Sketches Definition 6.68, DaoFP §14.9); reversing the inequalities gives *oplax* monoidal monotones.

> Sources: 7 Sketches Definition 2.41, Example 2.42, 2.65, Exercises 2.43–2.45, 2.68; Construction 2.64.

## Examples

- $i : (\mathbb{N}, \leq, 0, +) \to (\mathbb{R}, \leq, 0, +)$, $n \mapsto n$: strict.
- $\lfloor - \rfloor : (\mathbb{R}, \leq, 0, +) \to (\mathbb{N}, \leq, 0, +)$: monoidal monotone since $\lfloor x \rfloor + \lfloor y \rfloor \leq \lfloor x + y \rfloor$, but not strong: $\lfloor 0.5 \rfloor + \lfloor 0.5 \rfloor \neq \lfloor 1 \rfloor$.
- $g : \mathbf{Bool} \to \mathbf{Cost}$, $\mathsf{false} \mapsto \infty$, $\mathsf{true} \mapsto 0$: strict ([[7S Chapter 2 Exercises#Exercise 2.43|7S Exercise 2.43]]).
- $d, u : \mathbf{Cost} \to \mathbf{Bool}$, $d(x) = [x = 0]$ and $u(x) = [x < \infty]$: both strict ([[7S Chapter 2 Exercises#Exercise 2.44|7S Exercise 2.44]]); via [[Change of Base]] they turn [[Lawvere Metric Space|Lawvere metric spaces]] into preorders in two different ways ([[7S Chapter 2 Exercises#Exercise 2.68|7S Exercise 2.68]]).
- The constant map $n \mapsto 1$ is the unique monoidal monotone $(\mathbb{N}, \leq, 0, +) \to (\mathbb{N}, \leq, 1, \ast)$ ([[7S Chapter 2 Exercises#Exercise 2.45|7S Exercise 2.45]]).

## Use

A monoidal monotone $f : \mathcal{V} \to \mathcal{W}$ converts $\mathcal{V}$-categories into $\mathcal{W}$-categories ([[Change of Base]]): condition (a) makes identities work, (b) makes composition work. Monoidal preorders and monoidal monotones form a category; strong monoidal monotones that are isomorphisms of preorders are isomorphisms of monoidal preorders.

````tabs
tab: Julia
```julia
# check the lax monoidal conditions on finite samples
function is_monoidal_monotone(P, Q, f, ps)
  cond_a = leq(Q, munit(Q), f(munit(P)))
  cond_b = all(leq(Q, otimes(Q, f(p1), f(p2)), f(otimes(P, p1, p2))) for p1 in ps, p2 in ps)
  is_monotone(P, Q, f, ps) && cond_a && cond_b
end
g(b::Bool) = b ? 0.0 : Inf           # Bool → Cost
is_monoidal_monotone(BoolPre(), CostPre(), g, [false, true])   # true
```
tab: Lean
```lean
-- Mathlib: a monotone monoid hom between ordered monoids is a strict monoidal monotone
#check OrderMonoidHom          -- α →*o β : monoid hom that is also monotone
-- lax version, by hand:
structure LaxMonoidalMonotone (P Q : Type) [OrderedCommMonoid P] [OrderedCommMonoid Q] where
  toFun : P → Q
  mono : Monotone toFun
  unit : 1 ≤ toFun 1
  mul : ∀ p₁ p₂, toFun p₁ * toFun p₂ ≤ toFun (p₁ * p₂)
```
tab: Haskell
```haskell
-- a (lax) monoidal monotone: monotone, with mempty <= f mempty and f p <> f q <= f (p <> q)
newtype MonoidalMonotone p q = MonoidalMonotone (p -> q)

boolToCost :: All -> Cost
boolToCost (All True)  = Fin 0
boolToCost (All False) = Inf
```
````
