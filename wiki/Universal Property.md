#definition #annotation

A **universal property** characterizes an object by how it relates to *all* other objects of a given kind, rather than by internal structure: "the hallmark of universality is the existence of a unique map to (or from) any other comparable object" (7 Sketches §6.2.1). An object with a universal property is unique up to unique [[Isomorphism]], which is why one speaks of *the* product, *the* initial object, etc. (Remark 3.85, Remark 6.9, [[7S Chapter 6 Exercises#Exercise 6.10|7S Exercise 6.10]]). Universal constructions come in dual pairs (mapping-in vs. mapping-out) and, in DaoFP's programmer's language, as *introduction* and *elimination rules* whose interplay gives the computation ($\beta$) and uniqueness ($\eta$) rules.

> Sources: 7 Sketches §3.4 ("Universal constructions"), §6.2.1 (Exercise 6.8), Remarks 3.85, 6.9; DaoFP §1–3 (the Yoneda trick: two objects are isomorphic iff they have naturally isomorphic mapping-outs), §4–7, §9–10, §11.5 ($\beta$/$\eta$); Kittenlab Lecture 8 ("Representatives of functors").

| universal object | defined by | dual |
|---|---|---|
| [[Terminal Object]] | unique map in from every object | [[Initial Object]] |
| [[Product]] | pairs of maps in ↔ maps into product | [[Coproduct]] |
| [[Limit]] (pullback, equalizer, ...) | cones ↔ maps into the limit | [[Colimit]] (pushout, coequalizer) |
| [[Exponential Object]] | maps out of $a \times b$ ↔ maps into $c^b$ | (none in general) |
| [[Free Monoid]], free objects | maps out of the generators | cofree objects |
| [[Subobject Classifier]] | subobjects ↔ maps into $\Omega$ | |
| [[Initial Algebra]] | unique algebra map out (catamorphism) | [[Terminal Coalgebra]] (anamorphism) |
| [[Kan Extension]], [[End]], [[Coend]] | universal (co)wedges / factorizations | |

All of these are instances of a [[Representable Functor|representability]] statement $\mathcal{C}(x, u) \cong \Phi(x)$ (Kittenlab: "a universal property is a natural isomorphism with a hom-functor"), hence of the [[Yoneda Lemma]]; many are [[Adjunction|adjoints]] to simple functors, and Mac Lane's slogan says all are [[Kan Extension|Kan extensions]].
