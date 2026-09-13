#definition #example #theorem #proof

A **unital commutative quantale** is a [[Monoidal Closed Preorder|symmetric monoidal closed preorder]] $\mathcal{V} = (V, \leq, I, \otimes, \multimap)$ that has all [[Join|joins]]: $\bigvee A$ exists for every $A \subseteq V$. The empty join is denoted $0 := \bigvee \varnothing$ (the bottom element). In 7 Sketches "quantale" always means unital commutative quantale. The word is a portmanteau of *quantum locale*.

> Sources: 7 Sketches §2.5.2, Definition 2.90, Example 2.91, Remark 2.95, 2.97, Propositions 2.96, 2.98, Exercises 2.92–2.94; §2.6; [Ros90].

## Examples

| quantale | $\otimes$ | $I$ | $\bigvee$ | $0 = \bigvee\varnothing$ | $\multimap$ |
|---|---|---|---|---|---|
| [[Bool (Monoidal Preorder)|$\mathbf{Bool}$]] | $\wedge$ | $\mathsf{true}$ | OR | $\mathsf{false}$ | $\Rightarrow$ |
| [[Cost]] | $+$ | $0$ | $\inf$ (usual order) | $\infty$ — "beware!" | $\max(0, y - x)$ |
| $(\mathcal{P}(S), \subseteq, S, \cap)$ | $\cap$ | $S$ | $\bigcup$ | $\varnothing$ | $\overline{B} \cup C$ |
| $(\mathbb{N}, \leq, 1, \ast)$ (with $\infty$ added) | $\ast$ | $1$ | $\sup$ | $0$ | |
| $\mathcal{P}(M)$ for a monoid $M$; binary relations on a set under composition | | | | | noncommutative (§2.6) |

In $\mathbf{Cost}$ every $A \subseteq [0, \infty]$ has a join, its infimum in the usual order: $\bigvee \{2, 3\} = 2$, $\bigvee \{2.5, 2.05, \dots\} = 2$, $\bigvee \varnothing = \infty$ (Example 2.91, [[7S Chapter 2 Exercises#Exercise 2.92|7S Exercise 2.92]]).

## Theory

**Proposition 2.96.** A [[Preorder]] has all joins iff it has all meets. *Proof.* By duality it suffices to show joins give meets. For $A \subseteq P$ let $M_A := \{p \mid p \leq a \text{ for all } a \in A\}$ and $m_A := \bigvee M_A$. Then $m_A \leq a$ for all $a \in A$ (each $p \in M_A$ is $\leq a$, so is their join), and any lower bound $m'$ of $A$ lies in $M_A$, hence $m' \leq m_A$. $\blacksquare$ So a quantale has all meets too.

**Proposition 2.98.** A symmetric monoidal preorder with all joins is closed (hence a quantale) iff $\otimes$ distributes over joins, $v \otimes \bigvee A \cong \bigvee_{a \in A} v \otimes a$; then $v \multimap w = \bigvee \{a \mid a \otimes v \leq w\}$ ([[Adjoint Functor Theorem for Preorders]]).

**Remark 2.95 (the navigator).** A quantale personifies a *navigator*: given routes $A \to B$ and $B \to C$ they compose a route $A \to C$ (the monoidal product), and they search over way-points (the join). This is exactly what [[Matrix Multiplication in a Quantale]] does: $(M \ast N)(x, z) = \bigvee_y M(x,y) \otimes N(y,z)$, which computes the $\mathcal{V}$-category presented by a [[Weighted Graph]]. Generalized [[Hausdorff Distance]] also needs a quantale.

Noncommutative quantales (relations under composition, power sets of monoids) have applications to concurrency, process semantics and automata [AV93].

````tabs
tab: Julia
```julia
# a finite quantale given by elements, leq, otimes, munit; joins computed from leq
struct FiniteQuantale{T}
  elems::Vector{T}; leq::Function; otimes::Function; munit::T
end
join(Q::FiniteQuantale, A) = first(p for p in Q.elems if all(Q.leq(a, p) for a in A) && all(Q.leq(p, q) for q in Q.elems if all(Q.leq(a, q) for a in A)))
hom(Q::FiniteQuantale, v, w) = join(Q, [a for a in Q.elems if Q.leq(Q.otimes(a, v), w)])
is_quantale(Q) = all(Q.leq(Q.otimes(a, v), w) == Q.leq(a, hom(Q, v, w)) for a in Q.elems, v in Q.elems, w in Q.elems)

Bool_ = FiniteQuantale([false, true], (a,b) -> a <= b, (a,b) -> a && b, true)
is_quantale(Bool_)   # true
```
tab: Lean
```lean
-- Mathlib: `IsQuantale` (Mathlib/Algebra/Order/Quantale.lean): a complete lattice with a
-- semigroup structure distributing over sSup; unital commutative version with CommMonoid
#check IsQuantale
#check IsQuantale.leftResiduation   -- the hom-element x ⇨ₗ y
-- ENNReal (= Cost^op with the usual order) is a complete linear order whose + distributes over ⨆:
#check @ENNReal.add_iSup
```
tab: Haskell
```haskell
class Closed v => Quantale v where
  joinAll :: [v] -> v            -- must exist for every (finite, here) list; joinAll [] = bottom

instance Quantale All  where joinAll = foldr (\(All a) (All b) -> All (a || b)) (All False)
instance Quantale Cost where
  joinAll [] = Inf
  joinAll xs = foldr1 minC xs
    where minC (Fin a) (Fin b) = Fin (min a b); minC Inf b = b; minC a Inf = a
```
````
