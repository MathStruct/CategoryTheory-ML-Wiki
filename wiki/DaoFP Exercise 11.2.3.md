#exercise #solution #proof

**Exercise 11.2.3.** Show that a [[Pullback]] in $\mathcal{C}$ with target $b$ is a [[Product]] in the [[Slice Category]] $\mathcal{C}/b$.

## Solution

Let $\langle e, p \rangle$, $\langle e', p' \rangle$ be objects of $\mathcal{C}/b$ and $e \times_b e'$ their pullback with legs $\pi, \pi'$. It is an object of $\mathcal{C}/b$ via $p \circ \pi = p' \circ \pi'$, and $\pi, \pi'$ are slice morphisms (they commute with the projections by construction). Given a slice object $\langle x, q \rangle$ with slice morphisms $u : x \to e$, $u' : x \to e'$ — i.e. $p u = q = p' u'$ — the square commutes, so the pullback gives a unique $h : x \to e \times_b e'$ with $\pi h = u$, $\pi' h = u'$; $h$ is a slice morphism since $(p \pi) h = p u = q$. This is the universal property of the product in $\mathcal{C}/b$. (Kittenlab's "typed products" $A \times_T A'$ in $\mathbf{FinSet}/T$ are exactly this.)

> Sources: DaoFP Exercise 11.2.3; Kittenlab Lecture 13.
