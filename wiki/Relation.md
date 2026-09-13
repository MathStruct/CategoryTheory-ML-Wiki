#definition #example

Let $X$ and $Y$ be [[Set|sets]]. A **relation** between $X$ and $Y$ is a subset $R \subseteq X \times Y$. A **binary relation on** $X$ is a relation between $X$ and $X$, i.e. $R \subseteq X \times X$.

> Sources: 7 Sketches Definition 1.12, Example 1.13; Kittenlab Lecture 14 & 15; DaoFP §17.1 ("Profunctors as relations").

**Infix notation**: pick a symbol $\star$ and write $a \star b$ for $(a,b) \in R$. Examples: $\leq$ on $\mathbb{R}$ (so $5 \leq 6$ rather than $(5,6) \in R$), $=$, $\approx$, $<$, and divisibility $|$ in number theory ($5 \mid 10$).

## Special relations

- [[Function|Functions]] are relations $F \subseteq S \times T$ where each $s$ is related to exactly one $t$; the **graph** of a function $f$ is $\mathrm{graph}(f) = \{(x,y) \mid f(x) = y\}$, i.e. $\chi_{\mathrm{graph}(f)}(x,y) = [f(x) = y]$.
- [[Equivalence Relation|Equivalence relations]] are reflexive, symmetric, transitive binary relations.
- [[Preorder|Preorder relations]] are reflexive and transitive binary relations.
- All binary relations on $S$ form a preorder $\mathrm{Rel}(S)$ under inclusion; see [[Reflexive Transitive Closure]] for the Galois connection $\mathrm{Cl} \dashv U$ between $\mathrm{Rel}(S)$ and preorders on $S$.

## Relations as a joint constraint (Kittenlab)

A relation $R \subseteq X \times Y$ is a "joint constraint": knowing $x$ tells you something about $y$ and vice versa. Relations compose: for $R \subseteq X \times Y$ and $S \subseteq Y \times Z$,

$$
\chi_{S \circ R}(x, z) = [\exists y \in Y,\ \chi_R(x,y) \wedge \chi_S(y,z)],
$$

which is matrix multiplication with $(\vee, \wedge)$ in place of $(+, \cdot)$. This makes sets and relations into the [[Category of Relations|category $\mathbf{Rel}$]]. A relation is also a [[Span]] $X \leftarrow R \to Y$, and a $\mathbb{B}$-valued [[Profunctor]] ($\mathbf{Bool}$-profunctor / [[Feasibility Relation]]) once $X$, $Y$ are preorders.

````tabs
tab: Julia
```julia
# Kittenlab Lecture 14: finite relations as bit matrices
const FinRelation = BitMatrix
R = FinRelation([i < j for i in 1:4, j in 1:4])

function compose(R::FinRelation, S::FinRelation)
  n, m1, m2, l = (size(R)..., size(S)...)
  @assert m1 == m2
  FinRelation([any(R[i,j] && S[j,k] for j in 1:m1) for i in 1:n, k in 1:l])
end
compose(R, R)   # the relation i < j - 1

# Catlab: the category of finite sets and relations (FinRel)
using Catlab, Catlab.CategoricalAlgebra.FinRelations
R = FinRelation((x, y) -> x < y, 3, 3)   # relation given by a predicate
S = compose(R, R); S(1, 3)               # true
M = FinRelation(BoolRig.([true false; false true; true true]))  # by a boolean matrix
```
tab: Lean
```lean
-- Mathlib: a relation is a binary predicate; `Rel α β := α → β → Prop`
#check (Rel ℕ ℕ)
def divides : Rel ℕ ℕ := fun a b => ∃ k, b = a * k
-- relational composition
#check (Rel.comp : Rel α β → Rel β γ → Rel α γ)
-- the category of types and relations in Mathlib: `CategoryTheory.RelCat`
#check CategoryTheory.RelCat
```
tab: Haskell
```haskell
-- a relation as a predicate on pairs
type Rel a b = a -> b -> Bool

divides :: Rel Int Int
divides a b = b `mod` a == 0

-- composition over a finite middle set
compRel :: [b] -> Rel a b -> Rel b c -> Rel a c
compRel ys r s x z = any (\y -> r x y && s y z) ys

-- a relation as a list of pairs (a span)
type FinRel a b = [(a, b)]
```
````
