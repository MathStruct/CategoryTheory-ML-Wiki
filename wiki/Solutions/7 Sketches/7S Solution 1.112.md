#solution #proof

**Solution to [[7S Exercise 1.112|Exercise 1.112]].**

Let $f \dashv g$, $A \subseteq P$ with join $j$. Monotonicity gives $f(a) \leq f(j)$ for all $a \in A$, so $f(j)$ is an upper bound of $f(A)$. If $b$ is another upper bound, $f(a) \leq b$ for all $a$, so by adjunction $a \leq g(b)$ for all $a$, hence $j \leq g(b)$, hence $f(j) \leq b$. So $f(j) = \bigvee f(A)$.

> Sources: 7 Sketches, Exercise 1.112 and Solution A.1.
