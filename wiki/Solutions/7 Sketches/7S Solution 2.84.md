#solution #proof

**Solution to [[7S Exercise 2.84|Exercise 2.84]].**

Define $v \Rightarrow w$ by: $\mathsf{false} \Rightarrow w = \mathsf{true}$, $\mathsf{true} \Rightarrow w = w$. Then $(a \wedge v) \leq w$ iff $a \leq (v \Rightarrow w)$: if $v = \mathsf{false}$ both sides are always true; if $v = \mathsf{true}$ both sides say $a \leq w$.

> Sources: 7 Sketches, Exercise 2.84 and Solution A.2.
