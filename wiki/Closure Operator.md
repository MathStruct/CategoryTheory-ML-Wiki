#definition #example #theorem #proof

A **closure operator** $j : P \to P$ on a [[Preorder]] $P$ is a [[Monotone Map]] such that for all $p \in P$:

(a) $p \leq j(p)$ (*extensive*);
(b) $j(j(p)) \cong j(p)$ (*idempotent*).

> Sources: 7 Sketches §1.4.4, Definition 1.120, Examples 1.121–1.123, Exercise 1.119; §7.4.5 (modalities); DaoFP Ch. 14 (monads: a closure operator is a monad on a thin category).

## Closure operators from Galois connections

If $f \dashv g$ is a [[Galois Connection]] then $f \mathbin{;} g : P \to P$ is a closure operator ([[7S Chapter 1 Exercises#Exercise 1.119|7S Exercise 1.119]]): $p \leq g(f(p))$ is the unit inequality, and $g(f(g(f(p)))) \cong g(f(p))$ follows from $f(g(q)) \leq q$ with $q = f(p)$ applied inside $g$, together with extensivity. The other composite $g \mathbin{;} f$ is an [[Interior Operator]].

## Galois connections from closure operators (Example 1.122)

Let $\mathrm{fix}_j := \{p \in P \mid j(p) \cong p\}$, a sub-preorder of $P$; note every $j(p)$ is a fixed point. Then $j : P \to \mathrm{fix}_j$ is left adjoint to the inclusion $\iota : \mathrm{fix}_j \to P$. *Proof.* For $p \in P$, $q \in \mathrm{fix}_j$: if $j(p) \leq q$ then $p \leq j(p) \leq q$; conversely if $p \leq q$ then $j(p) \leq j(q) \cong q$. $\blacksquare$ So every closure operator arises from an adjunction — the preorder version of the fact that every [[Monad]] arises from an [[Adjunction]] via its [[Eilenberg-Moore Category|algebras]].

## Examples

- **Computation** (Example 1.121): expressions ordered by rewritability; a program $j : \mathsf{exp} \to \mathsf{exp}$ reducing expressions is a closure operator: monotone, $x \leq j(x)$ (only permissible rewrites), and $j(j(x)) = j(x)$ (reducing a reduced expression does nothing).
- **Logic / modal operators** (Example 1.123): propositions ordered by implication form a preorder; a closure operator is a **modal operator**, e.g. "assuming Bob is in San Diego, $-$" i.e. $B \Rightarrow -$: $p \Rightarrow (B \Rightarrow p)$ and $(B \Rightarrow (B \Rightarrow p)) \Rightarrow (B \Rightarrow p)$. See [[Modality]] in a [[Topos]].
- **[[Reflexive Transitive Closure]]** of a relation; topological closure; the transitive closure used to [[Join|join]] partitions.
- Closure operators on $\mathbb{B}$-categories generalize to [[Monad|monads]]; on [[Lawvere Metric Space|metric spaces]], to "closure" of subsets.

````tabs
tab: Julia
```julia
# a closure operator on the power set of {1..n}: closing a set under a relation R (reachability)
function closure(R::BitMatrix, U::BitVector)
  V = copy(U)
  changed = true
  while changed
    changed = false
    for i in findall(V), j in 1:length(V)
      if R[i, j] && !V[j]; V[j] = true; changed = true; end
    end
  end
  V
end
# laws: U ⊆ closure(R,U); closure(R, closure(R,U)) == closure(R,U); monotone in U
```
tab: Lean
```lean
#check ClosureOperator          -- structure: monotone, le_closure, idempotent
#check @ClosureOperator.closed  -- the fixed points
#check @ClosureOperator.gc      -- the Galois connection with the inclusion of fixed points
#check @GaloisConnection.closureOperator  -- u ∘ l from a Galois connection
```
tab: Haskell
```haskell
-- a closure operator as a function with laws (unenforced): x <= j x, j (j x) == j x, monotone
newtype Closure a = Closure (a -> a)

-- from a Galois connection
closureOf :: Galois a b -> Closure a
closureOf (Galois f g) = Closure (g . f)

-- example: reflexive-transitive closure of a relation (see that note)
```
````
