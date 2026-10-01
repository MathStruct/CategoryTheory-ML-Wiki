#paper

**Algebraic Data Integration** — Patrick Schultz & Ryan Wisnesky (2015; J. Funct. Programming 27 (2017)). [arXiv:1503.03571](https://arxiv.org/abs/1503.03571) (v8, [PDF](https://arxiv.org/pdf/1503.03571v8)).

The computational counterpart of *Algebraic Databases*: schemas and instances are presented as multi-sorted equational theories, instances denote their initial term algebras, and the three data-migration functors $\Sigma_F \dashv \Delta_F \dashv \Pi_F$ are computed syntactically. Adds a for/where/return query language and a pushout-based design pattern for data integration, implemented in the CQL tool, with decision procedures for equality in the equational theories involved.

> Sources: the paper, arXiv:1503.03571v8, checked against the arXiv listing. Index: [[Papers]].

## Key definitions and results

- §3: multi-sorted equational logic
- §4.1: type sides, schemas, instances, mappings, transforms
- §4.2: functorial data migration
- §4.3: uber-flower queries
- §5.1: deciding equality in equational theories
- §5.2: saturating theories into term models

## Concept notes

[[Algebraic Database]]

## Used in Sophia

[Graph Schema](https://mathstruct.org/Sophia/vault/Design/Graph-Schema)
