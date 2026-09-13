#solution

**Solution to [[7S Exercise 1.99|Exercise 1.99]].**

1. $f = (1 \mapsto 1, 2 \mapsto 1, 3 \mapsto 3)$, $g = (1 \mapsto 2, 2 \mapsto 2, 3 \mapsto 3)$. Checking all nine pairs, $f(p) \leq q$ iff $p \leq g(q)$ holds (for $(p,q) = (3,1), (3,2)$ both sides fail; otherwise both hold), so $f \dashv g$.
2. Here $f(2) = 1$ but $2 \not\leq g(1)$, so $f$ is *not* left adjoint to $g$. In pictures of [[Total Order|total orders]], adjoint pairs are exactly those whose bent arrows do not cross (Remark 1.100).

> Sources: 7 Sketches, Exercise 1.99 and Solution A.1.
