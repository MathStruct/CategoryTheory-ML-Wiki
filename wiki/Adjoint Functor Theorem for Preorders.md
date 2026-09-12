#theorem #proof

**Theorem 1.115.** Suppose $Q$ is a [[Preorder]] that has all [[Meet|meets]] and let $P$ be any preorder. A [[Monotone Map]] $g : Q \to P$ preserves meets if and only if it is a right adjoint (of a [[Galois Connection]]). Similarly, if $P$ has all [[Join|joins]] and $Q$ is any preorder, a monotone $f : P \to Q$ preserves joins iff it is a left adjoint.

> Sources: 7 Sketches Theorem 1.115; DaoFP §10.8 ("Freyd's theorem in a preorder", "Solution set condition"); general version: [[Adjoint Functor Theorem]].

*Proof (meets).* One direction is [[Right Adjoints Preserve Meets]]. Conversely suppose $g$ preserves meets. Define the candidate left adjoint by
$$f(p) := \bigwedge \{q \in Q \mid p \leq g(q)\}, \qquad (1.116)$$
which exists because $Q$ has all meets. *Monotone:* if $p \leq p'$ then $\{q' \mid p' \leq g(q')\} \subseteq \{q \mid p \leq g(q)\}$, so by Proposition 1.91 ([[Meet]]) $f(p) \leq f(p')$. By Proposition 1.107 it suffices to show $p_0 \leq g(f(p_0))$ and $f(g(q_0)) \leq q_0$. For the first,
$$p_0 \leq \bigwedge\{g(q) \mid p_0 \leq g(q)\} \cong g\Big(\bigwedge\{q \mid p_0 \leq g(q)\}\Big) = g(f(p_0)),$$
where the inequality holds because $p_0$ is below every element of the set, and the isomorphism is meet-preservation. For the second, $\{q_0\} \subseteq \{q \mid g(q_0) \leq g(q)\}$, so
$$f(g(q_0)) = \bigwedge\{q \mid g(q_0) \leq g(q)\} \leq \bigwedge\{q_0\} = q_0. \qquad\blacksquare$$

**DaoFP's view (§10.8).** In a preorder a right adjoint $g$ to $f$ must satisfy $f(p) = \bigwedge\{q \mid p \leq g(q)\}$ — the *limit of the comma category* $p \downarrow g$. Freyd's theorem says that for general categories the same formula works provided $\mathcal{C}$ is complete, $g$ preserves limits, and a *solution set condition* holds guaranteeing the limit is over a *small* diagram; in a preorder with all meets the condition is automatic. Applied to [[Continuation|defunctionalization]]: DaoFP uses the theorem to explain why arbitrary functions can be replaced by a "solution set" of data.

The theorem explains the slogan: a monotone map *does not have [[Generative Effect|generative effects]]* iff it is a left adjoint.

````tabs
tab: Lean
```lean
-- Mathlib: a meet-preserving map out of a complete lattice is a right adjoint
#check @GaloisConnection.of_iInf   -- hmm: see `OrderIso`/`sInf` based constructions
#check @sInf_le                    -- the ingredients of (1.116)
-- The candidate left adjoint (1.116):
def leftAdjCandidate {P Q : Type} [Preorder P] [CompleteLattice Q] (g : Q → P) (p : P) : Q :=
  sInf {q | p ≤ g q}
```
tab: Haskell
```haskell
-- (1.116) on finite preorders: build the left adjoint from a meet-preserving g
leftAdjoint :: (Preorder p, Preorder q) => [q] -> (q -> p) -> p -> Maybe q
leftAdjoint qs g p = meet qs [q | q <- qs, leq p (g q)]
```
````
