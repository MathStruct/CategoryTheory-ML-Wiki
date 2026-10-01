#paper

**Fixing Incremental Computation: Derivatives of Fixpoints, and the Recursive Semantics of Datalog** — Mario Alvarez-Picallo, Alex Eyers-Taylor, Michael Peyton Jones & C. -H. Luke Ong (2018; ESOP 2019). [arXiv:1811.06069](https://arxiv.org/abs/1811.06069) (v2, [PDF](https://arxiv.org/pdf/1811.06069v2)).

Introduces change actions and derivatives as an algebraic basis for incremental computation, derives semi-naïve evaluation for full Datalog (including negation and aggregates) as an instance, and shows how to differentiate the least-fixed-point operator itself, so that recursive queries can be maintained incrementally under changes to their inputs.

> Sources: the paper, arXiv:1811.06069v2, checked against the arXiv listing. Index: [[Papers]].

## Key definitions and results

- Definitions 1–2: change action; derivative
- Theorem 3: the chain rule
- Definitions 4–5, Proposition 6: complete change actions and minus operators
- Theorem 27: concrete derivatives of Datalog formulae (upward and downward)
- Theorem 39: incremental computation of least fixed points
- Definition 40, Theorem 43: derivatives of fixed points and of the least-fixed-point operator

## Concept notes

[[Change Action]], [[Least Fixed Point]]

## Used in Sophia

[Compilation as Query](https://mathstruct.org/Sophia/vault/Design/Compilation-as-Query), [State of the Art - Incremental Computation](https://mathstruct.org/Sophia/vault/State-of-the-Art/State-of-the-Art---Incremental-Computation)
