#definition #example

Let $(\mathcal M, \cdot, 1)$ be a (preordered) monoid of **grades**. A **graded monad** on $\mathcal C$ is a family of functors $T_m$, $m \in \mathcal M$, with a unit $\eta : \mathrm{Id} \Rightarrow T_1$ and multiplications

$$
\mu_{m,n} : T_m T_n \Longrightarrow T_{m \cdot n},
$$

satisfying the monad laws up to the monoid laws of the grades (and, for a preordered monoid, coercions $T_m \Rightarrow T_{m'}$ when $m \le m'$). A computation of type $T_m A$ returns an $A$ and performs "an effect of size $m$"; sequencing multiplies (adds) the grades. Equivalently, a graded monad is a lax monoidal functor $\mathcal M \to [\mathcal C, \mathcal C]$ ([[Lax Functor]]).

> Sources: Katsumata, *Parametric effect monads and semantics of effect systems*, POPL 2014; Fujii, Katsumata & Melliès, *Towards a formal theory of graded monads* (FoSSaCS 2016); Orchard, Petricek & Mycroft (2014), *The semantic marriage of monads and effects*; Atkey, *Syntax and Semantics of Quantitative Type Theory*, LICS 2018 (graded *types*); [[Monad]], [[Writer Monad]].

## Examples

- **Writer with a monoid of costs** is the simplest graded monad in disguise: $T_m A = A$ tagged with cost $m$; sequencing adds costs. Tracking cost *in the type* rather than in the value turns it into a graded monad.
- **Bounded nondeterminism**: $T_n A$ = multisets of at most $n$ elements of $A$; sequencing multiplies bounds.
- **Differential privacy**: $T_\varepsilon A$ = $\varepsilon$-differentially private mechanisms; composition adds privacy budgets (the sequential composition theorem).
- **Approximate computation / error bounds**: $T_\delta A$ = computations correct up to error $\delta$; sequencing adds (or otherwise combines) errors.

## Why it appears in categorical machine learning

Many learning algorithms are compositional *only approximately*, and the approximation composes: a mean-field inversion loses the mutual information between branches, a scalarised energy loses a Jensen gap, a stop-gradient drops Jacobian blocks. Annotating each piece with "how exact it is" and letting the annotations combine along composition is precisely a grading. A graded discipline would make "this composite is exact" a *type*, and refuse to call a composite of an exact and a lax piece exact — the bookkeeping [[Lax Functor|laxness]] asks an implementation to do.

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# A cost-graded computation: the grade is carried in the type parameter and adds under bind.
struct Graded{C,T}; value::T; end
Graded{C}(v) where {C} = Graded{C,typeof(v)}(v)
ret(v) = Graded{0}(v)
bind(m::Graded{C1}, f) where {C1} = (r = f(m.value); Graded{C1 + cost(r)}(r.value))
cost(::Graded{C}) where {C} = C
step1(x) = Graded{3}(x + 1)          # an operation of cost 3
step2(x) = Graded{5}(2x)             # an operation of cost 5
r = bind(bind(ret(1), step1), step2)
r.value, cost(r)                      # (4, 8): grades add along sequencing
```
tab: Lean
```lean
import Mathlib
-- A graded monad over a monoid of grades M (laws omitted).
class GradedMonad (M : Type) [Monoid M] (T : M → Type → Type) where
  pure {α : Type} : α → T 1 α
  bind {m n : M} {α β : Type} : T m α → (α → T n β) → T (m * n) β
```
tab: Haskell
```haskell
{-# LANGUAGE DataKinds, KindSignatures, TypeOperators #-}
import GHC.TypeLits

-- cost-graded computations: the grade is a type-level natural and adds under bind
newtype Cost (n :: Nat) a = Cost a deriving Show

ret :: a -> Cost 0 a
ret = Cost

bind :: Cost m a -> (a -> Cost n b) -> Cost (m + n) b
bind (Cost a) f = let Cost b = f a in Cost b

step1 :: Int -> Cost 3 Int
step1 x = Cost (x + 1)
step2 :: Int -> Cost 5 Int
step2 x = Cost (2 * x)
-- ret 1 `bind` step1 `bind` step2 :: Cost 8 Int
```
````
