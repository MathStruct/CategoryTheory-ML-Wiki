#solution #proof

**Solution to [[DaoFP Exercise 11.2.1|Exercise 11.2.1]].**

A cone over $b \to 1 \leftarrow e$ is a pair of arrows $q_1 : x \to b$, $q_2 : x \to e$ with $! \circ q_1 = ! \circ q_2$ — a condition that is automatic since there is only one arrow $x \to 1$. So a cone is just a pair of arrows, and the pullback's universal property (unique $h : x \to b \times_1 e$ with $\pi_1 h = q_1$, $\pi_2 h = q_2$) is exactly the universal property of $b \times e$. Fibrationally: pulling $e$ back along $! : b \to 1$ plants a copy of $e$ over every point of $b$ — the trivial bundle ([[Base Change Functor]]).

> Sources: DaoFP Exercise 11.2.1.
