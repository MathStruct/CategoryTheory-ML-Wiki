#paper

**Semantic foundations of equality saturation** — Dan Suciu, Yisu Remy Wang & Yihong Zhang (2025; ICDT 2025). [arXiv:2501.02413](https://arxiv.org/abs/2501.02413) (v1, [PDF](https://arxiv.org/pdf/2501.02413v1)).

Gives equality saturation a fixed-point semantics. An e-graph is a deterministic, reachable tree automaton; its meaning is a partial congruence relation, and every such relation has a unique e-graph. Equality saturation is the least fixed point of an inflationary, monotone operator and is a universal model of the rewrite rules. It corresponds exactly to a class of chase sequences from database theory, and termination is studied in three variants — all undecidable or worse in general — with an acyclicity criterion that guarantees it.

> Sources: the paper, arXiv:2501.02413v1, checked against the arXiv listing. Index: [[Papers]].

## Key definitions and results

- Definition 3: an e-graph is a deterministic reachable tree automaton
- Definition 5, Theorem 6: semantics is a partial congruence relation (PCR); PCRs ↔ e-graphs bijectively
- Lemma 11: at most one homomorphism between e-graphs
- Definition 13: an e-graph is a model of a term rewriting system
- Theorem 19: EqSat is the least fixed point of ICOR and a universal model
- Corollary 27: finite convergence
- Theorem 30: EqSat terminates iff the corresponding standard chase does
- Theorems 31–33: single-instance termination R.E.-complete; all-term-instance $\Pi_2$-complete; all-e-graph-instance undecidable

## Concept notes

[[E-Graph]], [[Least Fixed Point]]

## Used in Sophia

[Compilation as Query](https://mathstruct.org/Sophia/vault/Design/Compilation-as-Query)
