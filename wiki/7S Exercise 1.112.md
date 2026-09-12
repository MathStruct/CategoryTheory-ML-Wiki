#exercise #solution #proof

**Exercise 1.112.** Complete the proof of Proposition 1.111 by showing that left adjoints preserve [[Join|joins]].

> See [[Right Adjoints Preserve Meets]].

## Solution

Let $f \dashv g$, $A \subseteq P$ with join $j$. Monotonicity gives $f(a) \leq f(j)$ for all $a \in A$, so $f(j)$ is an upper bound of $f(A)$. If $b$ is another upper bound, $f(a) \leq b$ for all $a$, so by adjunction $a \leq g(b)$ for all $a$, hence $j \leq g(b)$, hence $f(j) \leq b$. So $f(j) = \bigvee f(A)$.
