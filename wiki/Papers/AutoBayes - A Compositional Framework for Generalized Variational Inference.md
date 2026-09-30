#paper

**AutoBayes: A Compositional Framework for Generalized Variational Inference** — Toby St Clere Smithe & Marco Perin (2025). [arXiv:2503.18608](https://arxiv.org/abs/2503.18608) (v2, [PDF](https://arxiv.org/pdf/2503.18608v2)).

Introduces a compositional framework in which approximate Bayesian inference is assembled from local pieces, like automatic differentiation. Models are [[Open Model|open models]] whose composition files intermediate variables into a latent space instead of integrating them out; attaching approximate inversions gives [[Bayesian Lens|Bayesian lenses]]; adding an energy and an entropy gives [[Statistical Game|statistical games]], whose losses (generalised free energies) satisfy a chain rule. Parameterized games and a lax composition of gradients complete the picture, and the appendix recovers maximum likelihood, EM, variational Bayesian EM, supervised learning and Bayesian deep learning as wirings.

> Sources: the paper, arXiv:2503.18608v2, checked against the arXiv listing. Index: [[Papers]].

## Key definitions and results

- Definitions 1–8: open models, sequential and parallel composition, copiers, cups and caps (self-dual compact closed)
- Definitions 9–12, Theorem 13: Bayesian lenses and the chain rule for open models
- Definition 15, Remark 16: parallel composition of inversions is lax
- Definition 17, Proposition 18: variational free energy in three forms
- Definition 20: statistical game; Definition 22 and Theorem 23: energies add, entropies chain, free energy obeys a chain rule
- Remark 24: priors as games; Remark 26: laxness = mutual information
- Definitions 27–29, Remark 30: parameterized games, composition of gradients, lax sections of a fibration
- Appendix A: Examples 1–5

## Concept notes

[[Open Model]], [[Bayesian Lens]], [[Bayesian Inversion]], [[Variational Free Energy]], [[Statistical Game]], [[Lax Functor]], [[Compact Closed Category]]

## Used in Lenticulum.jl

[Factors are Parameterized Statistical Games](https://mathstruct.org/Lenticulum.jl/dev/vault/Foundations/Factors-are-Parameterized-Statistical-Games) · [AutoBayes to Lenticulum](https://mathstruct.org/Lenticulum.jl/dev/vault/Foundations/AutoBayes-to-Lenticulum) · [Scalar and Multivariate Energy](https://mathstruct.org/Lenticulum.jl/dev/vault/Foundations/Scalar-and-Multivariate-Energy)
