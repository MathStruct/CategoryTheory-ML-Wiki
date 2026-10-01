#paper

**Computational category-theoretic rewriting** — Kristopher Brown, Evan Patterson, Tyler Hanks & James Fairbanks (2021; ICGT 2022). [arXiv:2111.03784](https://arxiv.org/abs/2111.03784) (v3, [PDF](https://arxiv.org/pdf/2111.03784v3)).

Implements graph rewriting at the level of generality of finitely presented C-sets in Julia (AlgebraicRewriting.jl, built on Catlab). Pushout complements, DPO, SPO, sesqui-pushout and PBPO+ rewriting are implemented once and inherited by every C-set schema, and further constructions (slices, structured cospans, distributed graphs) inherit efficient algorithms by relating their limits and colimits to those of C-sets.

> Sources: the paper, arXiv:2111.03784v3, checked against the arXiv listing. Index: [[Papers]].

## Key definitions and results

- §3.0.1: pushout complements; identification and dangling conditions for C-sets
- §3.0.2: DPO, SPO, SqPO and PBPO+ rewriting
- Proposition 1: spans of diagrams induce rewrite rules via the Yoneda embedding and colimits
- §4: implementation — homomorphism search for C-sets (4.1), diagrammatic syntax for rules (4.2), distributed graph rewriting (4.5), graph processes (4.6), further extensions (4.7)

## Concept notes

[[Double-Pushout Rewriting]], [[Attributed C-Set]]

## Used in Sophia

[Compilation as Query](https://mathstruct.org/Sophia/vault/Design/Compilation-as-Query), [Graph Schema](https://mathstruct.org/Sophia/vault/Design/Graph-Schema)
