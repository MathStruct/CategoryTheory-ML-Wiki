#paper

**Categorical Foundations of Gradient-Based Learning** — G. S. H. Cruttwell, Bruno Gavranović, Neil Ghani, Paul Wilson & Fabio Zanasi (2021). [arXiv:2103.01931](https://arxiv.org/abs/2103.01931) (v2, [PDF](https://arxiv.org/pdf/2103.01931v2)).

Shows that gradient-based learning — models, losses, learning rates, optimisers and the backward pass — can be described uniformly with three constructions: the [[Para Construction]] for parameters, [[Lens|lenses]] for bidirectional information flow, and [[Reverse Derivative Category|reverse derivative categories]] for differentiation. A training setup becomes a composite of parametric lenses; the same framework covers neural networks over $\mathbf{Smooth}$, Boolean circuits over $\mathbf{Poly}_{\mathbb Z_2}$, and deep dreaming, and it is accompanied by a Python implementation.

> Sources: the paper, arXiv:2103.01931v2, checked against the arXiv listing. Index: [[Papers]].

## Key definitions and results

- Definitions 2.1–2.3: $\mathbf{Para}(\mathcal C)$ and reparametrisation
- Definition 2.4: $\mathbf{Lens}(\mathcal C)$
- Definition 2.5: parametric lenses $\mathbf{Para}(\mathbf{Lens}(\mathcal C))$
- Definition 2.6 / A.5, Proposition 2.7: CRDCs and the functor $R : \mathcal C \to \mathbf{Lens}(\mathcal C)$
- Definitions 3.3, 3.8, 3.11, 3.14: loss map, learning rate, gradient update, stateful optimisers (momentum, Nesterov, Adagrad, Adam)
- §4: supervised learning, Boolean circuits, deep dreaming

## Concept notes

[[Para Construction]], [[Lens]], [[Parametric Lens]], [[Reverse Derivative Category]], [[Cartesian Differential Category]], [[Gradient-Based Learning with Parametric Lenses]]

## Used in Lenticulum.jl

[Lux as a Parametric Lens](https://mathstruct.org/Lenticulum.jl/dev/vault/Foundations/Lux-as-a-Parametric-Lens) · [Everything is a Factor](https://mathstruct.org/Lenticulum.jl/dev/vault/Factor-Graphs/Everything-is-a-Factor)
