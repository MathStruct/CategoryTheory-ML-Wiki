#exercise #solution #proof

**Exercise 2.20.** 1. Prove formally from the axioms of a [[Symmetric Monoidal Preorder]] that the assertions $t \leq v + w$, $w + u \leq x + z$, $v + x \leq y$ imply $t + u \leq y + z$. 2. Where are reflexivity and transitivity used? 3. Why is symmetry not needed for diagram (2.12)?

> See [[Wiring Diagrams for Monoidal Preorders]].

## Solution

1.
$$\begin{aligned} t + u &\leq (v + w) + u && \text{(monotonicity: } t \leq v + w,\ u \leq u) \\ &= v + (w + u) && \text{(associativity)} \\ &\leq v + (x + z) && \text{(monotonicity: } v \leq v,\ w + u \leq x + z) \\ &= (v + x) + z && \text{(associativity)} \\ &\leq y + z && \text{(monotonicity: } v + x \leq y,\ z \leq z). \end{aligned}$$
2. Reflexivity gives $u \leq u$, $v \leq v$, $z \leq z$; transitivity chains the inequalities into $t + u \leq y + z$.
3. No wires cross in the diagram, so symmetry is never invoked.
