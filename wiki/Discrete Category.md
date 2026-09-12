#definition #example

A **discrete category** is a [[Category]] with no morphisms other than identities. A discrete category is the same thing as a [[Set]] ("a bare-object category is a set: a category with no structure", DaoFP §8.1; 7 Sketches Example 3.74: the discrete category on a set is a left adjoint to the underlying set). The [[Discrete Preorder]] is the thin case.

> Sources: DaoFP §8.1, §10.11; 7 Sketches Example 3.74, 3.94, Exercise 3.83; Kittenlab Lecture 9.

- A [[Functor]] out of a discrete category is just a family of objects; a [[Diagram]] indexed by the discrete category with $n$ objects has as [[Limit]] the $n$-fold [[Product]] and as [[Colimit]] the $n$-fold [[Coproduct]] (Example 3.94; Kittenlab Lecture 9: "$n$-ary coproducts by making $\mathsf{D}$ the discrete category with $n$ objects").
- The discrete category on two objects has no [[Terminal Object]] ([[7S Exercise 3.83]]).
- $\mathrm{Disc} \dashv \mathrm{Ob} \dashv \mathrm{Codisc} : \mathbf{Set} \rightleftarrows \mathbf{Cat}$; see [[Codiscrete Category]].

````tabs
tab: Lean
```lean
#check CategoryTheory.Discrete      -- Discrete α: objects α, only identity morphisms
#check CategoryTheory.Discrete.functor   -- a family α → C gives Discrete α ⥤ C
```
````
