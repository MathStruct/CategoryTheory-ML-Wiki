#definition #example

Let $(P, \leq)$ be a [[Preorder]] and $A \subseteq P$. An element $p$ is a **join** (least upper bound) of $A$ if

(a) for all $a \in A$, $a \leq p$, and
(b) for all $q$ with $a \leq q$ for all $a \in A$, we have $p \leq q$.

We write $p = \bigvee A$ or $\bigvee_{a \in A} a$, and $a \vee b$ when $A = \{a, b\}$. Joins are the dual of [[Meet|meets]] (replace $\leq$ by $\geq$ everywhere), so all remarks there — uniqueness up to equivalence, possible non-existence, Proposition 1.91 ($A \subseteq B \Rightarrow \bigvee A \leq \bigvee B$) — dualize.

> Sources: 7 Sketches §1.1 ("joining systems"), Definition 1.81, Examples 1.87–1.89, Exercises 1.7, 1.85, 1.90, 1.94; Definition 1.93; CTfS §3.4.2 (Definition 3.4.2.1, Exercises 3.4.2.2–3.4.2.4), §3.4.4 (Exercises 3.4.4.3, 3.4.4.7, 3.4.4.10–3.4.4.11)

## Examples

- Joining systems: for [[Partition|partitions]], $A \vee B$ is the transitive closure of the union of connections — the smallest system bigger than both (§1.1.2, [[7S Chapter 1 Exercises#Exercise 1.6|7S Exercise 1.6]]).
- [[Booleans]]: join is OR; $\mathsf{true} \vee \mathsf{false} = \mathsf{true}$, $\mathsf{false} \vee \mathsf{false} = \mathsf{false}$ ([[7S Chapter 1 Exercises#Exercise 1.7|7S Exercise 1.7]]).
- [[Power Set]]: $A \vee B = A \cup B$. [[Total Order]]: supremum. [[Divisibility Order]]: $\mathrm{lcm}$.
- $\{\frac{1}{n+1} \mid n \in \mathbb{N}\} \subseteq \mathbb{R}$ has join $1$; $\mathbb{N} \subseteq \mathbb{R}$ has none.

## Joins in science (Category Theory for Scientists §3.4.4)

- **Taxonomy**: in the tree of life ordered by "is a kind of", the join of two species is their most specific common taxon; meets of distinct species usually do not exist ([[CTfS Chapter 3 Exercises#Exercise 3.4.4.3|CTfS Exercise 3.4.4.3]], [[Tree of Life]]).
- **Geography**: open regions of the earth ordered by inclusion have joins (unions) and binary meets (intersections). Assigning to each region the interval of temperatures recorded in it is a monotone map to intervals of $\mathbb R$ that preserves joins (the range over a union is the smallest interval containing both ranges) but not meets: the range over $U \cap V$ can be strictly smaller than the intersection of the two ranges ([[CTfS Chapter 3 Exercises#Exercise 3.4.4.11|CTfS Exercise 3.4.4.11]]).
- **Security**: the sets $K(I)$ of people who need to know every piece of information in $I$ reverse inclusion ($I_1 \subseteq I_2 \Rightarrow K(I_2) \subseteq K(I_1)$) and are closed under intersection, so they have meets; joins of such sets need not be unions ([[CTfS Chapter 3 Exercises#Exercise 3.4.4.7|CTfS Exercise 3.4.4.7]]).

## Joins and observations

For any monotone $f$ and $a, b$ with joins, $f(a) \vee f(b) \leq f(a \vee b)$ ([[7S Chapter 1 Exercises#Exercise 1.94|7S Exercise 1.94]]); strict inequality is a [[Generative Effect]]. Left adjoints of [[Galois Connection|Galois connections]] preserve joins ([[Right Adjoints Preserve Meets|and right adjoints preserve meets]]); a map out of a preorder with all joins is a left adjoint iff it preserves joins ([[Adjoint Functor Theorem for Preorders]]).

Categorically a join is a [[Colimit]] in the thin category: $a \vee b$ is the [[Coproduct]], $\bigvee \varnothing$ is the bottom element, an [[Initial Object]]. In a [[Quantale]] the monoidal product distributes over all joins.

````tabs
tab: Julia
**Docs:** [FinSets](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FinSets) · [C-set morphisms](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.CSets) · [Vignette: meets](https://algebraicjulia.github.io/Catlab.jl/v0.16/generated/sketches/meets/)
```julia
function join_in(leq, xs, A)
  ubs = [q for q in xs if all(leq(a, q) for a in A)]
  for p in ubs
    all(leq(p, q) for q in ubs) && return p
  end
  nothing
end
join_in((a,b) -> b % a == 0, 1:12, [4, 6])   # 12 (lcm)
```
Catlab version (run in a fresh Julia session — Catlab exports its own `compose`, `id`, `FinFunction`, …):
```julia
using Catlab
X = FinSet(3); U = Subobject(X, [1,2]); V = Subobject(X, [2,3])
join(U, V)   # {1,2,3}
```
tab: Lean
```lean
#check @IsLUB
#check @sSup
example (A B : Set ℕ) : A ⊔ B = A ∪ B := rfl
example (a b : Bool) : a ⊔ b = (a || b) := rfl
```
tab: Haskell
```haskell
join :: Preorder a => [a] -> [a] -> Maybe a
join xs as =
  let ubs = [q | q <- xs, all (`leq` q) as]
  in case [p | p <- ubs, all (leq p) ubs] of
       (p:_) -> Just p
       []    -> Nothing
```
````
