#definition #example

Every [[Set]] $X$ can be considered as a **discrete preorder** $(X, =)$: the only order relations are $x \leq x$; if $x \neq y$ neither $x \leq y$ nor $y \leq x$ holds. Its [[Hasse Diagram]] is a collection of points. It is already a [[Partial Order]].

> Sources: 7 Sketches Example 1.32, Exercises 1.41, 1.44, 1.55, 1.67, 1.73; Kittenlab Lecture 5.

- Two elements are comparable iff they are equal ([[7S Exercise 1.44]]).
- Every function out of a discrete preorder is [[Monotone Map|monotone]] ([[7S Exercise 1.67]]); thus $\mathrm{Disc} : \mathbf{Set} \to \mathbf{Preord}$ is a [[Functor]], left adjoint to the underlying-set functor (Kittenlab Lecture 5 lists it alongside the [[Codiscrete Preorder]] functor).
- The [[Upper Set|upper sets]] of a discrete preorder form the whole [[Power Set]] ([[7S Exercise 1.55]]).
- A skeletal [[Dagger Preorder]] is discrete, hence "can be identified with" a set ([[7S Exercise 1.73]], Remark 1.74).
- As a category, a discrete preorder is a [[Discrete Category]].
