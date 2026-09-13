#solution

**Solution to [[7S Exercise 2.63|Exercise 2.63]].**

Graph $A \xrightarrow{5} B$, $A \xrightarrow{10} C$, $B \xrightarrow{6} C$, $B \xrightarrow{10} A$, $C \xrightarrow{10} B$ gives

$$
\begin{pmatrix} \infty & 6 & 10 \\ 10 & \infty & 10 \\ 10 & 6 & \infty \end{pmatrix}.
$$

Diagonals equal the unit $\infty$ and $\min(M(x,y), M(y,z)) \leq M(x,z)$, so it is a $\mathbf{W}$-category. Interpretation: **weight limits** for trucking cargo — the hom-object is the maximum cargo weight allowed from $x$ to $y$; staying put has no limit; the limit $x \to z$ is at least $\min$ of the limits via $y$ (a "bottleneck" or max-min path problem).

> Sources: 7 Sketches, Exercise 2.63 and Solution A.2.
