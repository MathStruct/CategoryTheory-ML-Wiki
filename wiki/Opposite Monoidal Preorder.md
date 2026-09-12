#theorem #proof

**Proposition 2.38.** If $(X, \leq, I, \otimes)$ is a [[Symmetric Monoidal Preorder]], then so is its opposite $(X, \geq, I, \otimes)$, i.e. the [[Opposite Preorder]] with the same unit and product.

> Sources: 7 Sketches Proposition 2.38, Exercises 2.39, 2.40.

*Proof.* Monotonicity: suppose $x_1 \geq y_1$ and $x_2 \geq y_2$ in $X^{\mathrm{op}}$, i.e. $y_1 \leq x_1$ and $y_2 \leq x_2$ in $X$; monotonicity in $X$ gives $y_1 \otimes y_2 \leq x_1 \otimes x_2$, i.e. $x_1 \otimes x_2 \geq y_1 \otimes y_2$ in $X^{\mathrm{op}}$. Unitality and associativity are equations not involving the order. Symmetry $x \otimes y \cong y \otimes x$ holds in $X^{\mathrm{op}}$ because both inequalities hold in $X$ ([[7S Exercise 2.39]]). $\blacksquare$

**Example.** $\mathbf{Cost}^{\mathrm{op}} = ([0, \infty], \leq, 0, +)$ with the usual increasing order ([[7S Exercise 2.40]]). Compare the [[Opposite Category]] of a [[Monoidal Category]].
