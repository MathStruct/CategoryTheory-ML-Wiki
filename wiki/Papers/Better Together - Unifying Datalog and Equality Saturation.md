#paper

**Better Together: Unifying Datalog and Equality Saturation** — Yihong Zhang, Yisu Remy Wang, Oliver Flatt, David Cao, Philip Zucker, Eli Rosenthal, Zachary Tatlock & Max Willsey (2023; PLDI 2023). [arXiv:2304.04332](https://arxiv.org/abs/2304.04332) (v4, [PDF](https://arxiv.org/pdf/2304.04332v4)).

Presents **egglog**, a single fixpoint language that is both Datalog and equality saturation. Tables are functions with a functional dependency from arguments to result; a per-function `:merge` expression resolves conflicts. Taking the merge to be "union the two e-classes" recovers equality saturation (restoring the functional dependency is rebuilding), taking it to be a lattice join recovers Datalog with lattices and e-class analyses. Gives a least-fixed-point semantics and shows semi-naïve evaluation is correct.

> Sources: the paper, arXiv:2304.04332v4, checked against the arXiv listing. Index: [[Papers]].

## Key definitions and results

- §2.1: Datalog and its fixpoint semantics
- §3: functions, functional dependencies and `:merge`
- §4: semantics — immediate-consequence operator and rebuilding operator on pre-instances
- Theorem 4.1: semi-naïve evaluation of an egglog program produces the same database as naïve evaluation

## Concept notes

[[E-Graph]], [[Least Fixed Point]]

## Used in Sophia

[Compilation as Query](https://mathstruct.org/Sophia/vault/Design/Compilation-as-Query), [Graph Schema](https://mathstruct.org/Sophia/vault/Design/Graph-Schema)
