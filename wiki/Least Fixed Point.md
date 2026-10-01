#definition #theorem #algorithm #program

A **least fixed point** of a [[Monotone Map]] $f : L \to L$ on a partial order is an element $\mu f$ with $f(\mu f) = \mu f$ and $\mu f \le x$ for every other fixed point $x$. Two theorems guarantee that it exists and say how to compute it:

- **Knaster–Tarski.** If $L$ is a complete lattice, every monotone $f$ has a least fixed point, and it is the meet of all *pre-fixed points*: $\mu f = \bigwedge \{x \mid f(x) \le x\}$. Dually, the greatest fixed point is $\nu f = \bigvee \{x \mid x \le f(x)\}$.
- **Kleene.** If $f$ preserves joins of ascending chains (it is *Scott-continuous*) and $L$ has a least element $\bot$, then $\mu f = \bigvee_{n} f^n(\bot)$: iterate from the bottom until nothing changes.

Least fixed points are the semantics of **recursion that only accumulates** — reachability, Datalog, dataflow analysis, constraint-based type inference, [[E-Graph|equality saturation]]. Greatest fixed points are the semantics of **coinduction**: bisimilarity, infinite behaviour, [[Terminal Coalgebra|terminal coalgebras]].

> Sources: Tarski, *A lattice-theoretical fixpoint theorem and its applications*, Pacific J. Math. 5 (1955); van Emden & Kowalski, *The semantics of predicate logic as a programming language*, JACM 23 (1976) (least-model semantics of logic programs); Abiteboul, Hull & Vianu, *Foundations of Databases* (Addison-Wesley 1995), Chapter 12 (Datalog; naive and semi-naive evaluation); Abo Khamis, Ngo, Pichler, Suciu & Wang, *Convergence of Datalog over (Pre-) Semirings*, [arXiv:2105.14435](https://arxiv.org/abs/2105.14435) ([[Convergence of Datalog over (Pre-) Semirings|notes]]) Definitions 2.3, 3.1, Theorems 1.2, 6.4; Alvarez-Picallo, Eyers-Taylor, Peyton Jones & Ong, [arXiv:1811.06069](https://arxiv.org/abs/1811.06069) ([[Fixing Incremental Computation - Derivatives of Fixpoints, and the Recursive Semantics of Datalog|notes]]) Theorems 39, 43; Suciu, Wang & Zhang, [arXiv:2501.02413](https://arxiv.org/abs/2501.02413) ([[Semantic Foundations of Equality Saturation|notes]]) Theorem 19. Order-theoretic background: [[Galois Connection]], [[Closure Operator]] (7 Sketches Ch. 1).

## Datalog is a least fixed point

A Datalog program $P$ defines an **immediate-consequence operator** $T_P$ on database instances ordered by inclusion: one round of applying every rule to the current facts. $T_P$ is monotone (rules have no negation) and continuous (each derived fact uses finitely many premises), so by Kleene the program's meaning is $\mu T_P = \bigcup_n T_P^n(\emptyset)$, the **least Herbrand model**. Transitive closure is the standard example:

$$
\mathit{path}(x, z) \leftarrow \mathit{edge}(x, z), \qquad \mathit{path}(x, z) \leftarrow \mathit{path}(x, y), \mathit{edge}(y, z).
$$

**Semi-naïve evaluation** joins only the facts that are *new* in the last round and never re-derives old ones; it computes the same fixed point. Abo Khamis et al. prove this for any complete distributive dioid (Theorem 6.4), and Alvarez-Picallo et al. explain *why*: the semi-naïve delta is a derivative of $T_P$ in the sense of [[Change Action|change actions]], and iterating with a derivative computes the least fixed point of any differentiable continuous map (Theorem 39).

## Beyond sets: fixed points over semirings

Replace "set of facts" by "facts annotated with values in a semiring" and the same program computes shortest paths (the tropical semiring), counts derivations ($\mathbb N$), or records where an answer came from ([[Provenance Semiring]]). The order now comes from a **POPS**, a partially ordered pre-semiring (Definition 2.3), and the question becomes when the iteration converges. Abo Khamis et al. answer it exactly: every program converges iff the semiring $P \oplus \bot$ is **stable** — every $u$ satisfies $1 \oplus u \oplus \dots \oplus u^{p} = 1 \oplus u \oplus \dots \oplus u^{p+1}$ for some $p$ — and within a number of steps bounded by the active domain iff it is uniformly $p$-stable (Theorem 1.2). Over $\mathbb N$ a cycle diverges (infinitely many derivations); over the tropical semiring with non-negative weights it converges, because going round a cycle never helps.

## Where else it appears

| fixed point | of | computes |
|---|---|---|
| $\mu T_P$ | Datalog immediate consequence | least model: reachability, points-to analysis |
| $\mu\,\mathrm{ICOR}$ | rewrite-and-rebuild on e-graphs | equality saturation ([[E-Graph]]) |
| $\mu$ of a transfer function | an abstract domain (a lattice) | dataflow analysis, abstract interpretation |
| $\nu$ of a refinement step | relations on states | bisimilarity ([[Bisimulation]]) |
| $\mu X.\, F X$ | an endofunctor (categorified) | the [[Initial Algebra]], an inductive type |

The last row is the categorification. An initial algebra is a "least fixed point" of a functor, and Lambek's lemma ($F(\mu F) \cong \mu F$) is the analogue of $f(\mu f) = \mu f$; on a preorder regarded as a category, initial algebras *are* least fixed points.

## Sophia

Sophia's derived relations — transitive `DEPENDS_ON`, equivalence closure, reachability for garbage collection — are least fixed points, computed either as materialised tables or as Datalog rules, and its compiler is a saturation to a fixed point. Recompiling after an edit is semi-naïve evaluation of that fixed point. See [Compilation as Query](https://mathstruct.org/Sophia/vault/Design/Compilation-as-Query) and [Query Cookbook](https://mathstruct.org/Sophia/vault/Design/Query-Cookbook).

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# Datalog: path(x,z) :- edge(x,z).   path(x,z) :- path(x,y), edge(y,z).
edges = Set([(1, 2), (2, 3), (3, 4), (4, 2), (5, 1)])
# immediate-consequence operator T_P on the lattice of relations (sets of pairs), ordered by ⊆
T(path) = edges ∪ Set((x, z) for (x, y) in path for (y2, z) in edges if y == y2)
# naive (Kleene) iteration from ⊥ = ∅: ∅ ⊆ T(∅) ⊆ T²(∅) ⊆ … reaches the least fixed point
function naive(T)
    P, steps = Set{Tuple{Int,Int}}(), 0
    while (P2 = T(P)) != P; P, steps = P2, steps + 1; end
    P, steps
end
lfp, steps = naive(T)
T(lfp) == lfp                    # true: a fixed point
length(lfp)                      # 16 pairs: 5 reaches 1–4; 2, 3, 4 reach the whole cycle 2→3→4→2
# semi-naive: only join the *new* facts Δ with edge, never re-derive old ones
function seminaive()
    P = copy(edges); Δ = copy(edges); joins = 0
    while !isempty(Δ)
        new = Set((x, z) for (x, y) in Δ for (y2, z) in edges if y == y2)
        joins += length(Δ)
        Δ = setdiff(new, P); union!(P, Δ)
    end
    P, joins
end
P, joins = seminaive()
P == lfp                         # true: same least fixed point (the semi-naive theorem)
# every fixed point contains the least one: e.g. the full relation is a (pre)fixed point
full = Set((x, y) for x in 1:5, y in 1:5)
issubset(T(full), full) && issubset(lfp, full)   # true: lfp is below every pre-fixed point
```
tab: Lean
```lean
import Mathlib
-- Knaster–Tarski in Mathlib: every monotone map on a complete lattice has a least fixed point.
example {α : Type*} [CompleteLattice α] (f : α →o α) : f (OrderHom.lfp f) = OrderHom.lfp f :=
  OrderHom.map_lfp f

-- lfp is below every pre-fixed point: the induction principle for least fixed points
example {α : Type*} [CompleteLattice α] (f : α →o α) (a : α) (h : f a ≤ a) :
    OrderHom.lfp f ≤ a :=
  OrderHom.lfp_le f h

-- transitive closure as the least fixed point of a monotone operator on Set (α × α)
def stepT {α : Type*} (E : Set (α × α)) : Set (α × α) →o Set (α × α) where
  toFun P := E ∪ {p | ∃ y, (p.1, y) ∈ P ∧ (y, p.2) ∈ E}
  monotone' := fun _ _ h => Set.union_subset_union_right _ fun _ ⟨y, hy, he⟩ => ⟨y, h hy, he⟩

def transClosure {α : Type*} (E : Set (α × α)) : Set (α × α) := OrderHom.lfp (stepT E)
```
tab: Haskell
```haskell
import Data.List (nub, sort)

-- Kleene iteration of the Datalog immediate-consequence operator for transitive closure
edges :: [(Int, Int)]
edges = [(1, 2), (2, 3), (3, 4), (4, 2), (5, 1)]

step :: [(Int, Int)] -> [(Int, Int)]
step p = sort (nub (edges ++ [ (x, z) | (x, y) <- p, (y', z) <- edges, y == y' ]))

lfp :: Eq a => (a -> a) -> a -> a
lfp f x = let x' = f x in if x' == x then x else lfp f x'

main :: IO ()
main = print (length (lfp step []))   -- 16
```
````
