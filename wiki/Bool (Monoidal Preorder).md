#definition #example

$$
\mathbf{Bool} := (\mathbb{B}, \leq, \mathsf{true}, \wedge)
$$

is the [[Symmetric Monoidal Preorder]] on the [[Booleans]] with monoidal unit $\mathsf{true}$ and monoidal product AND. Identifying $\mathsf{false} = 0$, $\mathsf{true} = 1$, $\wedge$ is multiplication:

| $\wedge$ | $\mathsf{false}$ | $\mathsf{true}$ |
|---|---|---|
| $\mathsf{false}$ | $\mathsf{false}$ | $\mathsf{false}$ |
| $\mathsf{true}$ | $\mathsf{false}$ | $\mathsf{true}$ |

> Sources: 7 Sketches Example 2.27, Exercises 2.29, 2.84, 2.93, Theorem 2.49; DaoFP §20.1 ("the monoidal walking arrow"); Kittenlab Lecture 14.

**As a base of enrichment.** $\mathbf{Bool}$-categories are exactly [[Preorder|preorders]] ([[Preorders are Bool-Categories]]): the underlying set says "getting from $a$ to $b$ is a true/false question"; the unit $\mathsf{true}$ says "you can always get from $a$ to $a$"; the product $\wedge$ says "if you can get from $a$ to $b$ AND from $b$ to $c$ then from $a$ to $c$"; the "if–then" is the order $\leq$. DaoFP calls $\mathbb{B}$ the *walking arrow* $\mathsf{False} \to \mathsf{True}$ made monoidal by $\mathsf{True} \otimes \mathsf{True} = \mathsf{True}$, everything else $\mathsf{False}$.

**Properties.** $\mathbf{Bool}$ is [[Monoidal Closed Preorder|monoidal closed]] with hom-element implication $v \Rightarrow w$ ([[7S Chapter 2 Exercises#Exercise 2.84|7S Exercise 2.84]]) and is a [[Quantale]] with joins given by OR ([[7S Chapter 2 Exercises#Exercise 2.93|7S Exercise 2.93]]); the empty join is $\mathsf{false}$ ([[7S Chapter 2 Exercises#Exercise 2.92|7S Exercise 2.92]]). Its identity [[Matrix Multiplication in a Quantale|$\mathbf{Bool}$-matrix]] is the usual identity with $\mathsf{true}$ on the diagonal.

**The other structure.** $(\mathbb{B}, \leq, \mathsf{false}, \vee)$ is also a symmetric monoidal preorder ([[7S Chapter 2 Exercises#Exercise 2.29|7S Exercise 2.29]]), but it is *not* closed (Example 2.85): $\mathsf{false} \leq p \multimap q$ always, yet $(\mathsf{false} \vee \mathsf{true}) \not\leq \mathsf{false}$.

**Maps.** [[Monoidal Monotone Map|Monoidal monotones]] $\mathbf{Bool} \to \mathbf{Cost}$ ($\mathsf{false} \mapsto \infty$, $\mathsf{true} \mapsto 0$) and $\mathbf{Cost} \to \mathbf{Bool}$ ("is $x = 0$?", "is $x < \infty$?") connect preorders and [[Lawvere Metric Space|metric spaces]] via [[Change of Base]]. $\mathbf{Bool}$-[[Profunctor|profunctors]] are [[Feasibility Relation|feasibility relations]] (Chapter 4).

````tabs
tab: Julia
```julia
struct BoolPre <: Preorder{Bool} end
leq(::BoolPre, a::Bool, b::Bool) = a <= b       # false ≤ true
otimes(::BoolPre, a, b) = a && b
munit(::BoolPre) = true
hom(::BoolPre, v, w) = !v || w                  # v ⊸ w = (v ⇒ w)
join(::BoolPre, a, b) = a || b
```
tab: Lean
```lean
-- Bool is a Boolean algebra: ⊓ = and, ⊔ = or, ⇨ = implication (the closed structure)
example : BooleanAlgebra Bool := inferInstance
example (a v w : Bool) : (a ⊓ v ≤ w) ↔ (a ≤ v ⇨ w) := le_himp_iff
-- Prop is the "large" version: a Heyting algebra with → as internal hom
example (a v w : Prop) : (a ∧ v → w) ↔ (a → v → w) := ⟨fun h ha hv => h ⟨ha, hv⟩, fun h ⟨ha, hv⟩ => h ha hv⟩
```
tab: Haskell
```haskell
import Data.Monoid (All(..))
-- (Bool, &&, True) is the monoid All; with False <= True it is the monoidal preorder Bool
instance Preorder All where leq (All a) (All b) = a <= b
instance MonoidalPreorder All

implies :: Bool -> Bool -> Bool   -- the hom-element v ⊸ w
implies v w = not v || w
```
````
