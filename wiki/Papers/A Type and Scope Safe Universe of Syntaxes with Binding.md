#paper

**A Type and Scope Safe Universe of Syntaxes with Binding: Their Semantics and Proofs** — Guillaume Allais, Robert Atkey, James Chapman, Conor McBride & James McKinna (2020; extended version of the ICFP 2018 paper). [arXiv:2001.11001](https://arxiv.org/abs/2001.11001) (v2, [PDF](https://arxiv.org/pdf/2001.11001v2)).

Defines, in Agda, a universe of descriptions of syntaxes with binding, a single well-typed and well-scoped term type over all of them, and generic programs — renaming, substitution, printing, elaboration, normalisation by evaluation — together with generic proofs of their properties (fusion, simulation), written once and instantiated for every syntax.

> Sources: the paper, arXiv:2001.11001v2, checked against the arXiv listing. Index: [[Papers]].

## Key definitions and results

- §§2–3: primers on scope- and sort-safe terms and on type- and scope-safe programs
- §5: a universe of scope-safe, well-sorted syntaxes with binding
- §6: generic scope-safe programs; renaming and substitution as instances of one semantics
- §7: a catalogue of generic programs (printing, elaboration, normalisation by evaluation, …)
- §9: generic proofs about generic programs (simulation and fusion)

## Concept notes

[[Abstract Syntax with Binding]]

## Used in Sophia

[Hashing and Identity](https://mathstruct.org/Sophia/vault/Design/Hashing-and-Identity)
