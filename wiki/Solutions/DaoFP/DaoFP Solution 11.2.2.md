#solution #proof

**Solution to [[DaoFP Exercise 11.2.2|Exercise 11.2.2]].**

A diagram of that shape is a [[Cospan]] $f : A \to B \leftarrow C : g$. A [[Cone]] with apex $x$ consists of $q_a : x \to A$, $q_b : x \to B$, $q_c : x \to C$ with $f \circ q_a = q_b = g \circ q_c$; so $q_b$ is redundant and a cone is a pair $(q_a, q_c)$ with $f q_a = g q_c$ — a commuting square. The limit (terminal cone) is then an object $e'$ with $p' : e' \to A$, $h : e' \to C$ such that every such square factors uniquely through it: precisely the pullback.

> Sources: DaoFP Exercise 11.2.2.
