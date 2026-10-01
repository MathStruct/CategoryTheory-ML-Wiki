#paper

**Hashing Modulo Alpha-Equivalence** — Krzysztof Maziarz, Tom Ellis, Alan Lawrence, Andrew Fitzgibbon & Simon Peyton Jones (2021; PLDI 2021). [arXiv:2105.02856](https://arxiv.org/abs/2105.02856) (v1, [PDF](https://arxiv.org/pdf/2105.02856v1)).

Gives an algorithm that computes, for every subexpression of a λ-term, a hash such that two subexpressions get the same hash iff they are α-equivalent (up to hash collisions), in $O(n \log^2 n)$ time. It is based on a compositional *e-summary* (structure plus a variable map) that determines a term up to α exactly and can be hashed with weak combiners; the paper compares structural, de Bruijn and locally nameless hashing and bounds the collision probability.

> Sources: the paper, arXiv:2105.02856v1, checked against the arXiv listing. Index: [[Papers]].

## Key definitions and results

- §2.3–2.5: structural, de Bruijn and locally nameless hashing — cost, false positives and false negatives
- §4: e-summaries; invertibility; compositionality
- §5: hashed e-summaries
- Theorem 6.3: running time $O(|e| \log^2 |e|)$
- Definition 6.4: random functions
- Theorems 6.7–6.8: collision probability bounds; 128-bit hashes suffice for billion-node expressions
- §6.3: incrementality under local rewrites

## Concept notes

[[Abstract Syntax with Binding]]

## Used in Sophia

[Hashing and Identity](https://mathstruct.org/Sophia/vault/Design/Hashing-and-Identity)
