#exercise #solution #proof

**Exercise 2.84.** Show that $\mathbf{Bool} = (\mathbb{B}, \leq, \mathsf{true}, \wedge)$ is [[Monoidal Closed Preorder|monoidal closed]].

## Solution

Define $v \Rightarrow w$ by: $\mathsf{false} \Rightarrow w = \mathsf{true}$, $\mathsf{true} \Rightarrow w = w$. Then $(a \wedge v) \leq w$ iff $a \leq (v \Rightarrow w)$: if $v = \mathsf{false}$ both sides are always true; if $v = \mathsf{true}$ both sides say $a \leq w$.
