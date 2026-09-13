#definition #example

An **endofunctor** is a [[Functor]] $F : \mathcal{C} \to \mathcal{C}$ from a category to itself. All endofunctors of $\mathcal{C}$ compose, so they form a [[Monoid|monoid]] — indeed a strict [[Monoidal Category]] $[\mathcal{C}, \mathcal{C}]$ under composition, whose [[Monoid Object|monoids]] are [[Monad|monads]].

> Sources: DaoFP §8.3 ("Endofunctors"), Chapters 12–14; Kittenlab Lecture 7 (order-preserving maps $\mathbb{R} \to \mathbb{R}$ as endofunctors).

- In Haskell an endofunctor of $\mathbf{Hask}$ is a type constructor `f :: Type -> Type` with an `fmap`: `List`, `Maybe`, `Identity`, `Const c`, `WithInt` ([[DaoFP Chapter 8 Exercises#Exercise 8.3.1|DaoFP Exercise 8.3.1]]).
- [[Algebra of an Endofunctor|Algebras]] $F a \to a$ and [[Coalgebra of an Endofunctor|coalgebras]] $a \to F a$ of endofunctors define recursive data types ([[Initial Algebra]], [[Terminal Coalgebra]]).
- Every Haskell endofunctor is [[Functorial Strength|strong]] because $\mathbf{Hask}$ is self-[[Enriched Category|enriched]] (DaoFP §20.2).
