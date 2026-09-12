#definition #example #theorem

A **Lawvere metric space** is a [[Cost]]-[[Enriched Category|category]].

> Sources: 7 Sketches Definition 2.53, Examples 2.54, 2.72, 2.76, Exercises 2.55, 2.58, 2.73, 2.78; Remark 2.97; [Law73].

Unpacking: a $\mathbf{Cost}$-category $\mathcal{X}$ has a set $X := \mathrm{Ob}(\mathcal{X})$ of points and for each $x, y$ a hom-object $d(x, y) := \mathcal{X}(x, y) \in [0, \infty]$, satisfying

(a) $0 \geq d(x, x)$, i.e. $d(x, x) = 0$;
(b) $d(x, y) + d(y, z) \geq d(x, z)$, the triangle inequality.

Compared with a [[Metric Space]], symmetry and "$d(x,y) = 0 \Rightarrow x = y$" are dropped and infinite distances allowed — a "very compact definition that packs a punch". Examples: $\mathbb{R}$ with $|y - x|$; effort in hilly terrain; regions with the asymmetric [[Hausdorff Distance]]; any [[Weighted Graph|$\mathbf{Cost}$-weighted graph]] via shortest paths.

## Dictionary

| enriched notion | metric notion |
|---|---|
| $\mathbf{Cost}$-[[Enriched Functor|functor]] $F$ with $d_X(x_1, x_2) \geq d_Y(F x_1, F x_2)$ | 1-Lipschitz (distance non-increasing) map (Example 2.72) |
| skeletal dagger $\mathbf{Cost}$-category | extended metric space ([[7S Exercise 2.73]]) |
| [[Product of Enriched Categories|$\mathbf{Cost}$-product]] $X \times Y$ | $d((x,y),(x',y')) = d_X(x,x') + d_Y(y,y')$, the $\ell^1$ / Manhattan metric ([[7S Exercise 2.78]]: $d((5,6),(-1,4)) = 8$, not $\sqrt{40}$) |
| [[Change of Base]] along $[x = 0] : \mathbf{Cost} \to \mathbf{Bool}$ | the preorder "$x \leq y$ iff $d(x,y) = 0$", e.g. "is a part of" for regions ([[7S Exercise 2.67]]) |
| change of base along $[x < \infty]$ | the preorder "$y$ is reachable from $x$" |
| presentation by a $\mathbf{Cost}$-weighted graph | shortest-path distances, computed by [[Matrix Multiplication in a Quantale|min-plus matrix powers]] |
| $(\mathbb{R}_{\geq 0}, \geq, 0, +)$-category | finite-distance Lawvere metric space ([[7S Exercise 2.55]]) |
| $\mathbf{Cost}$-[[Profunctor|profunctor]] | a distance-like relation between two spaces (Chapter 4) |

Lawvere's paper [Law73] goes further, e.g. Cauchy completeness in categorical terms.

````tabs
tab: Julia
```julia
# a finite Lawvere metric space as a matrix of distances over Cost
X = VCategory(CostPre(), [:x, :y, :z], [0.0 4 3; 3 0 6; 7 4 0])   # Eq. (2.57)
is_vcategory(X)    # true: zeros on the diagonal, triangle inequality holds

# from a weighted graph via min-plus matrix powers (see Matrix Multiplication in a Quantale)
M = [0.0 4 3; 3 0 Inf; Inf 4 0]
minplus(A, B) = [minimum(A[i, k] + B[k, j] for k in axes(A, 2)) for i in axes(A, 1), j in axes(B, 2)]
d = minplus(M, M); minplus(d, M) == d   # stabilizes at the distance matrix
```
tab: Lean
```lean
-- The symmetric case is Mathlib's `PseudoEMetricSpace`; the general Lawvere case is a
-- category enriched in the monoidal (order-dual) ENNReal. By hand:
structure LawvereMetric (X : Type) where
  d : X → X → ENNReal
  d_self : ∀ x, d x x = 0
  triangle : ∀ x y z, d x z ≤ d x y + d y z
```
tab: Haskell
```haskell
-- a Lawvere metric space is a Cost-enriched category
type Lawvere o = VCat Cost o

realLawvere :: [Double] -> Lawvere Double
realLawvere pts = VCat pts (\x y -> Fin (abs (y - x)))
```
````
