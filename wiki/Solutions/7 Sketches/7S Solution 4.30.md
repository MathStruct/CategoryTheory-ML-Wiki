#solution #proof

**Solution to [[7S Exercise 4.30|Exercise 4.30]].**

1. (4.28): $\Phi(p,q) = I \otimes \Phi(p,q)$ (unitality); $\leq P(p,p) \otimes \Phi(p,q)$ (monotonicity of $\otimes$ with $I \leq P(p,p)$); $\leq \bigvee_{p_1} P(p,p_1) \otimes \Phi(p_1,q)$ (a join bounds each term); $= (U_P \mathbin{;} \Phi)(p,q)$ (definition).
2. In $\mathbf{Bool}$, $I = \mathsf{true}$ is top, so $P(p,p) = \mathsf{true}$ and the first inequality is an equality. For the second: if $\Phi(p,q) = \mathsf{true}$ equality is forced; if $\mathsf{false}$, then whenever $P(p, p_1) = \mathsf{true}$ monotonicity gives $\Phi(p_1, q) \leq \Phi(p, q) = \mathsf{false}$, so every term of the join is $\mathsf{false}$.
3. (4.29): $v \otimes I = v$; $I \leq Q(q,q)$ with monotonicity; the profunctor inequality of [[7S Exercise 4.9]].

> Sources: 7 Sketches, Exercise 4.30 and Solution A.4.
