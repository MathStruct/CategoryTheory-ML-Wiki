#paper

**egg: Fast and Extensible Equality Saturation** — Max Willsey, Chandrakana Nandi, Yisu Remy Wang, Oliver Flatt, Zachary Tatlock & Pavel Panchekha (2020; POPL 2021). [arXiv:2004.03082](https://arxiv.org/abs/2004.03082) (v3, [PDF](https://arxiv.org/pdf/2004.03082v3)).

Makes equality saturation practical with two ideas. **Rebuilding** defers the restoration of the e-graph's congruence and hashcons invariants until after a batch of rewrites, which amortises the work and gives large speedups over eager maintenance. **E-class analyses** attach semilattice-valued facts to e-classes and keep them consistent under merges, a general mechanism for constant folding, free-variable analysis and similar domain knowledge. Implemented as the Rust library *egg* and evaluated on several case studies.

> Sources: the paper, arXiv:2004.03082v3, checked against the arXiv listing. Index: [[Papers]].

## Key definitions and results

- Definition 2.1: e-graph $(U, M, H)$ — union-find, e-class map, hashcons
- Definitions 2.2–2.3: canonicalisation; when an e-graph represents a term
- Definitions 2.4–2.5: equivalence and congruence
- Definitions 2.6–2.7: the congruence and hashcons invariants
- §3: rebuilding (deferred invariant maintenance)
- §4: e-class analyses — `make`, `join`, `modify` over a join-semilattice, and the analysis invariant

## Concept notes

[[E-Graph]], [[Congruence]]

## Used in Sophia

[Compilation as Query](https://mathstruct.org/Sophia/vault/Design/Compilation-as-Query), [Equivalence and Witnesses](https://mathstruct.org/Sophia/vault/Design/Equivalence-and-Witnesses)
