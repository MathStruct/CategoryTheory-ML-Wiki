#solution

**Solution to [[7S Exercise 2.61|Exercise 2.61]].**

A set of points with, for each pair $(x, y)$, a value $\mathcal{X}(x,y) \in \{\mathsf{no}, \mathsf{maybe}, \mathsf{yes}\}$ — whether it is possible to get from $x$ to $y$ — such that $\mathcal{X}(x,x) = \mathsf{yes}$ and $\min(\mathcal{X}(x,y), \mathcal{X}(y,z)) \leq \mathcal{X}(x,z)$: it is at least as possible to go $x \to z$ directly as via $y$.

> Sources: 7 Sketches, Exercise 2.61 and Solution A.2.
