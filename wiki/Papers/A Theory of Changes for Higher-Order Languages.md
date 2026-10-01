#paper

**A Theory of Changes for Higher-Order Languages - Incrementalizing λ-Calculi by Static Differentiation** — Yufei Cai, Paolo G. Giarrusso, Tillmann Rendel & Klaus Ostermann (2013; PLDI 2014). [arXiv:1312.0658](https://arxiv.org/abs/1312.0658) (v1, [PDF](https://arxiv.org/pdf/1312.0658v1)).

Defines change structures and a static differentiation transformation for the simply typed λ-calculus: given a program, derive a program that maps input changes to output changes. Function types carry change structures, so higher-order programs can be incrementalised, and the transformation is proved correct with a change semantics.

> Sources: the paper, arXiv:1312.0658v1, checked against the arXiv listing. Index: [[Papers]].

## Key definitions and results

- Definition 2.1: change structures
- Theorem 2.7: function spaces carry change structures
- Definitions 3.1–3.8: domains, environments, evaluation, change semantics, erasure
- Theorem 3.11: correctness of differentiation

## Concept notes

[[Change Action]]

## Used in Sophia

[State of the Art - Incremental Computation](https://mathstruct.org/Sophia/vault/State-of-the-Art/State-of-the-Art---Incremental-Computation)
