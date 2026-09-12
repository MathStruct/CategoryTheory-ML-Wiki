#definition #theorem #example

A **modality** (modal operator, Lawvere–Tierney topology) in a sheaf topos $\mathbf{Shv}(X)$ is a sheaf morphism $j : \Omega \to \Omega$ such that for all opens $U \subseteq X$ and $p, q \in \Omega(U)$:

(a) $p \leq j(p)$;
(b) $j(j(p)) \leq j(p)$ (hence $j(j(p)) = j(p)$, [[7S Exercise 7.70]]);
(c) $j(p \wedge q) = j(p) \wedge j(q)$.

That is, $j$ is a [[Closure Operator]] on each poset of truth values $\Omega(U)$ (and each [[Heyting Algebra]] of [[Predicate|predicates]] $|\Omega^S|$) that preserves finite [[Meet|meets]] — a *nucleus*.

> Sources: 7 Sketches §7.4.5, Definition 7.69, Proposition 7.71, Exercises 7.70, 7.72; §1.4.4, Example 1.123 (modal operators as closure operators); §7.5.3 (the modality $@_t$).

**Proposition 7.71.** For a fixed proposition $p \in |\Omega|$, each of the following is a modality:
(a) $q \mapsto (p \Rightarrow q)$ — "assuming $p$, ...";
(b) $q \mapsto p \vee q$ — "... or $p$" (the closed modality);
(c) $q \mapsto (q \Rightarrow p) \Rightarrow p$ — for $p = \mathsf{false}$ this is double negation $\neg\neg$.

Example ([[7S Exercise 7.72]]): with $S$ the sheaf of people and $j$ = "assuming Bob is in San Diego", $j(p(s))$ is the set of times at which either Bob is not in San Diego or $s$ likes the weather; $p(s) \leq j(p(s))$, $j$ is idempotent, and $j$ preserves $\wedge$.

In [[Temporal Logic|temporal logic]] on the [[Topos of Behavior Types]], $@_t(q)$ — "$q$ holds in some small enough neighbourhood of time $t$" — is a modality of type (c).

````tabs
tab: Julia
```julia
# modalities on the finite Heyting algebra of opens of a small space: check (a)–(c) by brute force
Op = [BitSet(), BitSet([1]), BitSet([1, 2])]                   # Sierpiński opens
imp(u, v) = reduce(union, (r for r in Op if issubset(intersect(r, u), v)); init=BitSet())
ismodality(j) = all(issubset(p, j(p)) && j(j(p)) == j(p) for p in Op) &&
  all(j(intersect(p, q)) == intersect(j(p), j(q)) for p in Op, q in Op)
p0 = BitSet([1])
ismodality(q -> imp(p0, q))                 # (a) "assuming p": true
ismodality(q -> union(p0, q))               # (b) "or p": true
ismodality(q -> imp(imp(q, p0), p0))        # (c): true
```
tab: Lean
```lean
import Mathlib
#check @Nucleus                    -- a nucleus on a frame: inflationary, idempotent, meet-preserving
#check @Nucleus.idempotent
#check @Nucleus.le_apply
#check @le_compl_compl             -- a ≤ ¬¬a: double negation is inflationary
#check @compl_compl_compl          -- ¬¬¬a = ¬a: hence idempotent
```
tab: Haskell
```haskell
-- modalities on a finite Heyting algebra (as a list of elements with meet/implication)
isModality :: Eq h => [h] -> (h -> h -> h) -> (h -> h -> Bool) -> (h -> h) -> Bool
isModality els meet leq j =
     and [ p `leq` j p && j (j p) == j p | p <- els ]
  && and [ j (meet p q) == meet (j p) (j q) | p <- els, q <- els ]
```
````
