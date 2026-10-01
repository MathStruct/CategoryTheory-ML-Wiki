#paper

**Convergence of Datalog over (Pre-) Semirings** — Mahmoud Abo Khamis, Hung Q. Ngo, Reinhard Pichler, Dan Suciu & Yisu Remy Wang (2021; PODS 2022). [arXiv:2105.14435](https://arxiv.org/abs/2105.14435) (v5, [PDF](https://arxiv.org/pdf/2105.14435v5)).

Extends Datalog to programs over partially ordered pre-semirings (POPS), covering shortest paths, aggregates and provenance, with a least-fixed-point semantics. Characterises exactly when every program converges (the semiring with bottom adjoined is stable) and when it converges in a number of steps bounded by the active domain, and gives a semi-naïve algorithm that is correct over complete distributive dioids.

> Sources: the paper, arXiv:2105.14435v5, checked against the arXiv listing. Index: [[Papers]].

## Key definitions and results

- Definition 2.3: partially ordered pre-semiring (POPS)
- Definition 3.1: $p$-stable monotone function; stability index
- Theorem 1.2: convergence iff $P \oplus \bot$ is stable; bounded convergence iff uniformly $p$-stable
- Theorem 3.4: stability of tuples of functions in a clone
- Theorems 5.10, 5.12: polynomial functions over (p-)stable semirings
- Theorem 6.4: semi-naïve = naïve over complete distributive dioids
- Theorem 6.5: the differential rule

## Concept notes

[[Least Fixed Point]], [[Provenance Semiring]]

## Used in Sophia

[Compilation as Query](https://mathstruct.org/Sophia/vault/Design/Compilation-as-Query), [Query Cookbook](https://mathstruct.org/Sophia/vault/Design/Query-Cookbook)
