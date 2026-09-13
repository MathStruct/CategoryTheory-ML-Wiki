#definition #example

Let $X = (X, \leq_X)$ and $Y = (Y, \leq_Y)$ be [[Preorder|preorders]]. A **feasibility relation for $X$ given $Y$** is a [[Monotone Map]]

$$
\Phi : X^{\mathrm{op}} \times Y \to \mathbf{Bool}, \qquad\text{written } \Phi : X \nrightarrow Y.
$$

If $\Phi(x, y) = \mathsf{true}$ we say **$x$ can be obtained given $y$**. Monotonicity says: if $x' \leq_X x$ and $y \leq_Y y'$ then $\Phi(x, y) \leq \Phi(x', y')$ — if $x$ can be obtained given $y$, then anything less ($x'$) can be obtained given anything more ($y'$). A feasibility relation is exactly a $\mathbf{Bool}$-[[Profunctor]] ([[7S Exercise 4.10]]), and Censi calls them *monotone co-design problems*.

> Sources: 7 Sketches §4.2.1 (Definition 4.2, Exercises 4.4, 4.7, 4.10), §4.2.3, §4.3 ($\mathbf{Feas}$), Example 4.11 (bridges), 4.12; Kittenlab Lecture 14 ([[Relation|relations]] as $\mathbb{B}$-valued functions).

| $\mathbf{Bool}$-enriched notion | order-theoretic notion |
|---|---|
| [[Enriched Category|$\mathbf{Bool}$-category]] | preorder |
| [[Enriched Functor|$\mathbf{Bool}$-functor]] | monotone map |
| $\mathbf{Bool}$-profunctor | feasibility relation |

## Bridges (Example 4.11)

Think of the preorders as **cities** (Hasse diagrams: an arrow $A \to B$ is a way to get from $A$ to $B$) and the profunctor as **bridges** between them. $\Phi(x, y) = \mathsf{true}$ iff one can get from $x$ to $y$ using paths within the cities and the bridges: for the pictured $\Phi : X \nrightarrow Y$ with $W \leq N$, $b \leq a$ and bridges $N \to e$, $E \to a$, …, $\Phi(N, e) = \Phi(E, a) = \mathsf{true}$ but $\Phi(W, d) = \mathsf{false}$. The whole picture, boxed, is a new preorder — the [[Collage]] $\mathrm{Col}(\Phi)$. The matrix of values $\Phi(m, n) \in \mathbb{B}$ is the **feasibility matrix** ([[7S Exercise 4.12]]), and composition of feasibility relations is [[Matrix Multiplication in a Quantale|$\mathbf{Bool}$-matrix multiplication]].

## Properties

- The preimage $\Phi^{-1}(\mathsf{true})$ is an [[Upper Set]] of $X^{\mathrm{op}} \times Y$ ([[7S Exercise 4.4]]: "my aunt can explain a category given this book, hence a monoid given this book, and a category given nothing").
- Feasibility relations compose via $(\Phi \mathbin{;} \Psi)(p, r) = \bigvee_q \Phi(p, q) \wedge \Psi(q, r)$ — "the navigator searches $Q$ for a way-point" — forming the category $\mathbf{Feas} = \mathbf{Prof}_{\mathbf{Bool}}$ ([[Category of Profunctors]]), which is [[Compact Closed Category|compact closed]] with $X^* = X^{\mathrm{op}}$ and monoidal product the [[Product Preorder]]: $\Phi \times \Psi$ is "provide both $x_1$ and $y_1$ given both $x_2$ and $y_2$" ([[7S Exercise 4.64]]).
- Every monotone map $F : P \to Q$ gives feasibility relations $\hat F(p, q) = [F(p) \leq q]$ ([[Companion and Conjoint|companion]]) and $\check F(q, p) = [q \leq F(p)]$ (conjoint); e.g. $\widehat{+}(a, b, c, d) = [a + b + c \leq d]$ for $+ : \mathbb{R}^3 \to \mathbb{R}$ (Example 4.37).
- Interpretation of $\mathbf{Bool}$'s [[Quantale]] structure: $\wedge$ composes bridges, $\vee$ searches way-points, and $\Rightarrow$ (with $b \wedge c \leq d$ iff $b \leq c \Rightarrow d$, [[7S Exercise 4.7]]) is the hom-element. "It is the fact that $\mathbf{Bool}$ is a quantale which makes everything in this chapter work."

````tabs
tab: Julia
```julia
# a feasibility relation between finite preorders as a Bool matrix, with the monotonicity check
struct Feas
  X::Vector; leqX::Function        # resources produced
  Y::Vector; leqY::Function        # resources required
  Φ::Matrix{Bool}                  # Φ[i, j] = "X[i] can be obtained given Y[j]"
end
function is_feasibility(F::Feas)
  all(!(F.leqX(F.X[i′], F.X[i]) && F.leqY(F.Y[j], F.Y[j′]) && F.Φ[i, j]) || F.Φ[i′, j′]
      for i in eachindex(F.X), i′ in eachindex(F.X), j in eachindex(F.Y), j′ in eachindex(F.Y))
end
# movie example: T×E = (mean,boring) ≤ (mean,funny),(g/n,boring) ≤ (g/n,funny); $ = 100K ≤ 500K ≤ 1M
TE = [(:mean, :boring), (:mean, :funny), (:gn, :boring), (:gn, :funny)]
leqTE(a, b) = (a[1] == b[1] || a[1] == :mean) && (a[2] == b[2] || a[2] == :boring)
D = [100, 500, 1000]; leqD(a, b) = a <= b
Φ = Bool[1 1 1; 0 0 1; 0 1 1; 0 0 0]          # (g/n, funny) is infeasible at any cost
is_feasibility(Feas(TE, leqTE, D, leqD, Φ))   # true
```
tab: Lean
```lean
-- a feasibility relation is a monotone map Xᵒᵈ × Y → Bool
def FeasRel (X Y : Type) [Preorder X] [Preorder Y] := (Xᵒᵈ × Y) →o Bool
-- unfolding: Φ x y = true → x' ≤ x → y ≤ y' → Φ x' y' = true
```
tab: Haskell
```haskell
-- a feasibility relation as a Bool-valued function, monotone contravariantly in x, covariantly in y
newtype Feas x y = Feas (x -> y -> Bool)
-- law: leq x' x && leq y y' && phi x y  ==>  phi x' y'

-- the companion of a monotone map: "F p is available given q"
companion :: Preorder y => (x -> y) -> Feas x y
companion f = Feas (\p q -> leq (f p) q)
```
````
