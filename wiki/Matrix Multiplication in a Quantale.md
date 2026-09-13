#definition #example #theorem #program

Let $\mathcal{V} = (V, \leq, I, \otimes, \multimap)$ be a [[Quantale]]. A **$\mathcal{V}$-matrix** with rows $X$ and columns $Y$ is a function $M : X \times Y \to V$; $M(x, y)$ is the $(x,y)$-entry. The **product** of $M : X \times Y \to V$ and $N : Y \times Z \to V$ is $M \ast N : X \times Z \to V$,

$$
(M \ast N)(x, z) := \bigvee_{y \in Y} M(x, y) \otimes N(y, z), \qquad (2.101)
$$

joins standing in for $\sum$ and $\otimes$ for $\cdot$ in the usual formula. The **identity matrix** $I_X(x, y) := I$ if $x = y$ and $0 := \bigvee\varnothing$ otherwise.

> Sources: 7 Sketches §2.5.3, Definition 2.100, Example 2.102, Exercises 2.103–2.105; Kittenlab Lecture 14 (relation composition "looks suspiciously like matrix multiplication"); Chapter 4 ([[Profunctor|profunctor]] composition is exactly this).

**Example 2.102** ($\mathcal{V} = \mathbf{Bool}$, $X = Z = \underline{3}$, $Y = \underline{2}$):

$$
\begin{pmatrix} \mathsf{f} & \mathsf{f} \\ \mathsf{f} & \mathsf{t} \\ \mathsf{t} & \mathsf{t} \end{pmatrix} \ast \begin{pmatrix} \mathsf{t} & \mathsf{t} & \mathsf{f} \\ \mathsf{t} & \mathsf{f} & \mathsf{t} \end{pmatrix} = \begin{pmatrix} \mathsf{f} & \mathsf{f} & \mathsf{f} \\ \mathsf{t} & \mathsf{f} & \mathsf{t} \\ \mathsf{t} & \mathsf{t} & \mathsf{t} \end{pmatrix}.
$$

Identity matrices ([[7S Exercise 2.103]]): $\begin{pmatrix} 1 & 0 \\ 0 & 1\end{pmatrix}$ in $(\mathbb{N}, \leq, 1, \ast)$, $\begin{pmatrix} \mathsf{t} & \mathsf{f} \\ \mathsf{f} & \mathsf{t}\end{pmatrix}$ in $\mathbf{Bool}$, $\begin{pmatrix} 0 & \infty \\ \infty & 0\end{pmatrix}$ in $\mathbf{Cost}$.

**Laws** ([[7S Exercise 2.104]]): $I_X \ast M = M$ and $(M \ast N) \ast P = M \ast (N \ast P)$, using $0 \otimes v = v \otimes \bigvee \varnothing = \bigvee \varnothing = 0$ and distributivity of $\otimes$ over joins (Proposition 2.87). Hence sets and $\mathcal{V}$-matrices form a category — $\mathcal{V}$-$\mathbf{Mat}$, the category of $\mathcal{V}$-[[Profunctor|profunctors]] between discrete $\mathcal{V}$-categories, and for $\mathcal{V} = \mathbf{Bool}$ the [[Category of Relations]].

## Computing presented $\mathcal{V}$-categories

For a [[Weighted Graph]] $G$ with matrix $M_G$ (unit on the diagonal, weight on edges, $0 = \bigvee\varnothing$ elsewhere), the power $M_G^n$ records the best path using $\leq n$ edges; for finite vertex sets the powers **stabilize**, and the stable power is the hom-matrix of the [[Enriched Category|$\mathcal{V}$-category]] presented by $G$ (in the infinite case, take $\bigvee_n M^n$). For $\mathbf{Cost}$ this is the min-plus (tropical) computation of shortest-path distances: $M_Y^2 = M_Y^3 = d_Y$ (§2.5.3); for the graph $X$, $M_X^4 = M_X^3 = d_X$ ([[7S Exercise 2.105]]). For $\mathbf{Bool}$ it is the [[Reflexive Transitive Closure]] (Warshall's algorithm).

````tabs
tab: Julia
```julia
# generic quantale matrix multiplication
function qmul(V, M, N)
  [join(V, [otimes(V, M[i, k], N[k, j]) for k in axes(M, 2)]) for i in axes(M, 1), j in axes(N, 2)]
end
qid(V, n) = [i == j ? munit(V) : join(V, []) for i in 1:n, j in 1:n]

# Example 2.102 in Bool
join(::BoolPre, xs) = any(xs; init=false)
M = Bool[0 0; 0 1; 1 1]; N = Bool[1 1 0; 1 0 1]
qmul(BoolPre(), M, N)         # [0 0 0; 1 0 1; 1 1 1]

# Cost: shortest paths by powers
MY = [0.0 4 3; 3 0 Inf; Inf 4 0]
qmul(CostPre(), MY, MY)       # [0 4 3; 3 0 6; 7 4 0] = d_Y

# Catlab: Bool-matrices are FinRelations, composed by `compose`
using Catlab.CategoricalAlgebra.FinRelations
```
tab: Lean
```lean
-- Mathlib matrices over a semiring; (Bool, ∨, ∧) and the tropical semiring (min, +) are semirings
#check Matrix.mul
#check Tropical                    -- Tropical (WithTop ℝ): + is min, * is +
example : Semiring (Tropical (WithTop ℝ)) := inferInstance
-- Bool-matrix multiplication = relation composition:
example : Semiring Bool := inferInstance    -- ∨ as +, ∧ as *
```
tab: Haskell
```haskell
-- matrix multiplication over a quantale
qmul :: Quantale v => [[v]] -> [[v]] -> [[v]]
qmul m n = [ [ joinAll (zipWith (<>) row col) | col <- cols ] | row <- m ]
  where cols = foldr (zipWith (:)) (repeat []) n

qid :: Quantale v => Int -> [[v]]
qid k = [ [ if i == j then mempty else joinAll [] | j <- [1..k] ] | i <- [1..k] ]

-- powers stabilize at the hom-matrix of the presented V-category
closure :: (Quantale v, Eq v) => [[v]] -> [[v]]
closure m = go m where go d = let d' = qmul d m in if d' == d then d else go d'
```
````
