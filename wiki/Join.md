#definition #example

Let $(P, \leq)$ be a [[Preorder]] and $A \subseteq P$. An element $p$ is a **join** (least upper bound) of $A$ if

(a) for all $a \in A$, $a \leq p$, and
(b) for all $q$ with $a \leq q$ for all $a \in A$, we have $p \leq q$.

We write $p = \bigvee A$ or $\bigvee_{a \in A} a$, and $a \vee b$ when $A = \{a, b\}$. Joins are the dual of [[Meet|meets]] (replace $\leq$ by $\geq$ everywhere), so all remarks there — uniqueness up to equivalence, possible non-existence, Proposition 1.91 ($A \subseteq B \Rightarrow \bigvee A \leq \bigvee B$) — dualize.

> Sources: 7 Sketches §1.1 ("joining systems"), Definition 1.81, Examples 1.87–1.89, Exercises 1.7, 1.85, 1.90, 1.94; Definition 1.93.

## Examples

- Joining systems: for [[Partition|partitions]], $A \vee B$ is the transitive closure of the union of connections — the smallest system bigger than both (§1.1.2, [[7S Exercise 1.6]]).
- [[Booleans]]: join is OR; $\mathsf{true} \vee \mathsf{false} = \mathsf{true}$, $\mathsf{false} \vee \mathsf{false} = \mathsf{false}$ ([[7S Exercise 1.7]]).
- [[Power Set]]: $A \vee B = A \cup B$. [[Total Order]]: supremum. [[Divisibility Order]]: $\mathrm{lcm}$.
- $\{\frac{1}{n+1} \mid n \in \mathbb{N}\} \subseteq \mathbb{R}$ has join $1$; $\mathbb{N} \subseteq \mathbb{R}$ has none.

## Joins and observations

For any monotone $f$ and $a, b$ with joins, $f(a) \vee f(b) \leq f(a \vee b)$ ([[7S Exercise 1.94]]); strict inequality is a [[Generative Effect]]. Left adjoints of [[Galois Connection|Galois connections]] preserve joins ([[Right Adjoints Preserve Meets|and right adjoints preserve meets]]); a map out of a preorder with all joins is a left adjoint iff it preserves joins ([[Adjoint Functor Theorem for Preorders]]).

Categorically a join is a [[Colimit]] in the thin category: $a \vee b$ is the [[Coproduct]], $\bigvee \varnothing$ is the bottom element, an [[Initial Object]]. In a [[Quantale]] the monoidal product distributes over all joins.

````tabs
tab: Julia
```julia
function join(leq, xs, A)
  ubs = [q for q in xs if all(leq(a, q) for a in A)]
  for p in ubs
    all(leq(p, q) for q in ubs) && return p
  end
  nothing
end
join((a,b) -> b % a == 0, 1:12, [4, 6])   # 12 (lcm)

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
