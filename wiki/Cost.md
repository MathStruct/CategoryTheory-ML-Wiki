#definition #example

**Lawvere's monoidal preorder** is

$$
\mathbf{Cost} := ([0, \infty], \geq, 0, +),
$$

the non-negative reals together with $\infty$, ordered by $\geq$ (so $\infty \geq x$ for all $x$; the order is the *opposite* of the usual one), with monoidal unit $0$ and product $+$ ($x + \infty = \infty$).

> Sources: 7 Sketches Example 2.37, 2.54, 2.83, 2.91, Exercises 2.40, 2.55, 2.92, 2.103; Definition 2.53.

**As a base of enrichment.** $\mathbf{Cost}$ "structures the question of getting from here to there" as a question of *cost*: [[Enriched Category|$\mathbf{Cost}$-categories]] are [[Lawvere Metric Space|Lawvere metric spaces]]. The unit $0$ says you can always get from $a$ to $a$ at no cost; the product $+$ says the cost from $a$ to $c$ is *at most* the cost from $a$ to $b$ plus $b$ to $c$; the "at most" is the $\geq$. $\mathbf{Cost}$-[[Enriched Functor|functors]] are 1-Lipschitz maps; $\mathbf{Cost}$-[[Profunctor|profunctors]] appear in Chapter 4.

**Properties.**
- $\mathbf{Cost}^{\mathrm{op}} = ([0, \infty], \leq, 0, +)$ ([[7S Exercise 2.40]]).
- Monoidal closed with $x \multimap y := \max(0, y - x)$, "truncated subtraction" defined purely from order and product (Example 2.83): $a + x \geq y$ iff $a \geq y - x$ iff $a \geq \max(0, y-x)$.
- A [[Quantale]]: every $A \subseteq [0, \infty]$ has a join, namely its *infimum* in the usual order (Example 2.91); the empty join is $\infty$ — so the "$0$" of Definition 2.90 is $\infty$ here, "beware!" ([[7S Exercise 2.92]]). $x \vee y = \min(x, y)$.
- Identity $\mathbf{Cost}$-matrix: $0$ on the diagonal, $\infty$ off it ([[7S Exercise 2.103]]). [[Matrix Multiplication in a Quantale|Matrix multiplication]] $\bigvee_y M(x,y) + N(y,z) = \min_y (M(x,y) + N(y,z))$ is the min-plus (tropical) product that computes shortest paths.
- Dropping $\infty$ gives $(\mathbb{R}_{\geq 0}, \geq, 0, +)$, whose categories are "finite-distance Lawvere metric spaces" ([[7S Exercise 2.55]]).

````tabs
tab: Julia
```julia
struct CostPre <: Preorder{Float64} end
leq(::CostPre, x, y) = x >= y                   # reversed order; Inf is the bottom
otimes(::CostPre, x, y) = x + y
munit(::CostPre) = 0.0
hom(::CostPre, x, y) = max(0.0, y - x)           # x ⊸ y
join(::CostPre, xs) = isempty(xs) ? Inf : minimum(xs)
```
tab: Lean
```lean
-- ENNReal = [0, ∞] with the usual ≤ (Cost is its order dual) and truncated subtraction
#check ENNReal
example (x y : ENNReal) : ENNReal := x + y
example (x y : ENNReal) : ENNReal := y - x          -- max(0, y - x): tsub, the hom-element
#check @ENNReal.sub_le_iff_le_add                   -- b - a ≤ c ↔ b ≤ c + a  (adjunction)
example : CompleteLinearOrder ENNReal := inferInstance   -- all joins/meets: a quantale
```
tab: Haskell
```haskell
-- Cost = [0,∞] with reversed order, unit 0, product +
data Cost = Fin Double | Inf deriving (Eq, Show)
plus :: Cost -> Cost -> Cost
plus (Fin a) (Fin b) = Fin (a + b)
plus _ _ = Inf
instance Preorder Cost where
  leq _ Inf = True                 -- Inf is ≤-least: x ≥ Inf never, Inf ≥ x always
  leq Inf _ = False
  leq (Fin a) (Fin b) = a >= b     -- reversed
homCost :: Cost -> Cost -> Cost    -- x ⊸ y = max(0, y - x)
homCost (Fin x) (Fin y) = Fin (max 0 (y - x))
homCost Inf _ = Fin 0
homCost _ Inf = Inf
```
````
