#paper

**Backprop as Functor: A compositional perspective on supervised learning** — Brendan Fong, David I. Spivak & Rémy Tuyéras (2017). [arXiv:1711.10455](https://arxiv.org/abs/1711.10455) (v3, [PDF](https://arxiv.org/pdf/1711.10455v3)); LICS 2019.

Defines learners $(P, I, U, r)$ — parameter set, implementation, update and request — and shows that gradient descent with backpropagation is a strong symmetric monoidal functor from parametrised functions to learners, so training a composite network equals composing the learners of its layers.

> Sources: the paper, arXiv:1711.10455v3, checked against the arXiv listing. Index: [[Papers]].

## Key definitions and results

- Definition II.1: learners; Proposition II.4: the symmetric monoidal category Learn
- Definition III.1: Para
- Theorem III.2: $L_{\varepsilon,e} : \mathbf{Para} \to \mathbf{Learn}$ is a faithful strong symmetric monoidal functor

## Concept notes

[[Backprop as Functor]], [[Parametric Lens]], [[Para Construction]]
