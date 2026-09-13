#example

See [[Category of Finite Sets]] and [[Prop]]: $\mathbf{FinSet}$ with objects $\mathbb{N}$, morphisms functions $\underline{m} \to \underline{n}$, and monoidal product the disjoint union of functions

$$
(f + g)(i) = \begin{cases} f(i) & 1 \leq i \leq m \\ m' + g(i - m) & m + 1 \leq i \leq m + n \end{cases} \qquad (5.4)
$$

is a prop (7 Sketches Example 5.3, [[7S Chapter 5 Exercises#Exercise 5.5|7S Exercise 5.5]]). Its sub-prop $\mathbf{Bij}$ of bijections is the [[Free Prop]] on no generators; the prop functor $\mathbf{FinSet} \to \mathbf{Rel}$ sends $f$ to its graph (Example 5.12). $\mathbf{FinSet}$ is the prop presented by a commutative [[Monoid Object]] (functions $\underline{m} \to \underline{n}$ are built from $\underline{2} \to \underline{1}$, $\underline{0} \to \underline{1}$ and permutations), just as $\mathbf{Cospan}_{\mathbf{FinSet}}$ is presented by a special commutative [[Frobenius Monoid]].
