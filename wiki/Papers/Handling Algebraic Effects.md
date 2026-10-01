#paper

**Handling Algebraic Effects** — Gordon D. Plotkin & Matija Pretnar (2013; Logical Methods in Computer Science 9(4) (2013)). [arXiv:1312.1399](https://arxiv.org/abs/1312.1399) (v2, [PDF](https://arxiv.org/pdf/1312.1399v2)).

Introduces effect handlers for algebraic effects: a handler is a model of the effect theory, and handling a computation is the unique homomorphism from the free model. Gives a call-by-push-value calculus with handlers, its denotational semantics and reasoning principles, and shows that deciding whether a handler is correct (a model of the theory) is undecidable in general.

> Sources: the paper, arXiv:1312.1399v2, checked against the arXiv listing. Index: [[Papers]].

## Key definitions and results

- Definition 4.2: model of an effect theory
- Proposition 4.3: free models — the forgetful functor has a left adjoint
- Definition 4.4: products of models
- Definition 4.5: correct handlers
- §5: reasoning about handlers; call-by-push-value equations
- Theorems 6.2, 6.5, 6.6: correctness is $\Pi_2$-complete, $\Sigma_1$-complete, decidable for decidable theories

## Concept notes

[[Algebraic Effects and Handlers]], [[Call-by-Push-Value]]

## Used in Sophia

[Effects Memory and Resources](https://mathstruct.org/Sophia/vault/Design/Effects-Memory-and-Resources), [Core Calculus](https://mathstruct.org/Sophia/vault/Design/Core-Calculus)
