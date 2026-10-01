#paper

**Relational E-Matching** — Yihong Zhang, Yisu Remy Wang, Max Willsey & Zachary Tatlock (2021; POPL 2022). [arXiv:2108.02290](https://arxiv.org/abs/2108.02290) (v2, [PDF](https://arxiv.org/pdf/2108.02290v2)).

Observes that e-matching — finding all instances of a pattern in an e-graph — is a conjunctive query over a relational encoding of the e-graph (one table per function symbol). Structural and equality constraints both become joins, so worst-case optimal join algorithms apply, giving the first worst-case optimal e-matching algorithm and large speedups on patterns with shared variables.

> Sources: the paper, arXiv:2108.02290v2, checked against the arXiv listing. Index: [[Papers]].

## Key definitions and results

- Definitions 1–4: terms, patterns, equivalence and congruence relations, e-classes and e-nodes
- Definition 6: representation
- Definitions 7–8: e-matching substitutions and the e-matching problem
- §2: conjunctive queries, the AGM bound, generic join
- Theorem 9: relational e-matching is worst-case optimal
- Theorem 10: running time bound in terms of query output and relation sizes

## Concept notes

[[E-Graph]], [[Congruence]], [[Conjunctive Query]]

## Used in Sophia

[Compilation as Query](https://mathstruct.org/Sophia/vault/Design/Compilation-as-Query), [Query Cookbook](https://mathstruct.org/Sophia/vault/Design/Query-Cookbook)
