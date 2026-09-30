#definition #example

A **discrete category** is a [[Category]] with no morphisms other than identities. A discrete category is the same thing as a [[Set]] ("a bare-object category is a set: a category with no structure", DaoFP §8.1; 7 Sketches Example 3.74: the discrete category on a set is a left adjoint to the underlying set). The [[Discrete Preorder]] is the thin case.

> Sources: DaoFP §8.1, §10.11; 7 Sketches Example 3.74, 3.94, Exercise 3.83; Kittenlab Lecture 9; CTfS Example 4.1.2.32, Exercises 4.1.2.33–4.1.2.34, 4.3.2.6, 4.5.1.14, Example 4.5.3.10

- A [[Functor]] out of a discrete category is just a family of objects; a [[Diagram]] indexed by the discrete category with $n$ objects has as [[Limit]] the $n$-fold [[Product]] and as [[Colimit]] the $n$-fold [[Coproduct]] (Example 3.94; Kittenlab Lecture 9: "$n$-ary coproducts by making $\mathsf{D}$ the discrete category with $n$ objects").
- The discrete category on two objects has no [[Terminal Object]] ([[7S Chapter 3 Exercises#Exercise 3.83|7S Exercise 3.83]]).
- **Counting** (CTfS Exercises 4.1.2.33, 4.3.2.6): the discrete category $D_4$ on $\underline 4$ has exactly 4 morphisms (the identities); functors $D_3 \to D_2$ are just functions $\underline 3 \to \underline 2$ (there are 8), and there are no non-identity natural transformations between them, so the functor category $\mathrm{Fun}(D_3, D_2)$ is itself discrete with 8 objects. In a discrete category a product $x \times y$ exists iff $x = y$ (CTfS Exercise 4.5.1.14).
- $\mathrm{Disc} \dashv \mathrm{Ob} \dashv \mathrm{Codisc} : \mathbf{Set} \rightleftarrows \mathbf{Cat}$; see [[Codiscrete Category]].

````tabs
tab: Lean
```lean
#check CategoryTheory.Discrete      -- Discrete α: objects α, only identity morphisms
#check CategoryTheory.Discrete.functor   -- a family α → C gives Discrete α ⥤ C
```
````
