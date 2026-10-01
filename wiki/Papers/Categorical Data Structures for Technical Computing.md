#paper

**Categorical Data Structures for Technical Computing** — Evan Patterson, Owen Lynch & James Fairbanks (2021; Compositionality 4 (2022)). [arXiv:2106.04703](https://arxiv.org/abs/2106.04703) (v5, [PDF](https://arxiv.org/pdf/2106.04703v5)).

Introduces attributed C-sets (acsets), the data structure underlying Catlab.jl: C-sets extended with attributes of fixed type. Shows acsets form a slice category of C-sets, so that limits, colimits, homomorphism search and data migration are available generically, and describes an efficient implementation in Julia with typed, schema-generated code.

> Sources: the paper, arXiv:2106.04703v5, checked against the arXiv listing. Index: [[Papers]].

## Key definitions and results

- Definitions 1–4: C-sets, free categories, finitely presented categories
- Proposition 1: (co)limits in functor categories are pointwise
- Definitions 5–7: schema, acset (main and alternative definitions)
- Theorem 2: acsets on a schema form a slice category $\mathbf{Set}^{\mathcal C}/D$
- Propositions 3, 5, Corollary 6: colimits and limits of acsets
- Definitions 8–9: cospans and structured cospans of acsets

## Concept notes

[[Attributed C-Set]], [[Algebraic Database]]

## Used in Sophia

[Graph Schema](https://mathstruct.org/Sophia/vault/Design/Graph-Schema)
