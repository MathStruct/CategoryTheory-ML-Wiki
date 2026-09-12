#exercise #solution

**Exercise 4.4.** Let $X = \{\mathsf{monoid}, \mathsf{preorder} \leq \mathsf{category}\}$ and $Y = \{\mathsf{nothing} \leq \mathsf{this\ book}\}$. 1. Draw the Hasse diagram of $X^{\mathrm{op}} \times Y$. 2. Give a profunctor $\Lambda : X \nrightarrow Y$, reading $\Lambda(x, y) = \mathsf{true}$ as "my aunt can explain an $x$ given $y$", and interpret the fact that $\Lambda^{-1}(\mathsf{true})$ is an [[Upper Set]].

## Solution

1. Six elements: $(\mathsf{category}, \mathsf{nothing})$ at the bottom, then $(\mathsf{monoid}, \mathsf{nothing})$, $(\mathsf{preorder}, \mathsf{nothing})$, $(\mathsf{category}, \mathsf{this\ book})$, then $(\mathsf{monoid}, \mathsf{this\ book})$, $(\mathsf{preorder}, \mathsf{this\ book})$ on top — tasks in *decreasing difficulty*.
2. E.g. $\Lambda = \mathsf{true}$ on $(\mathsf{monoid}, -)$, $(\mathsf{preorder}, \mathsf{this\ book})$, $(\mathsf{category}, \mathsf{this\ book})$ and $\mathsf{false}$ on $(\mathsf{preorder}, \mathsf{nothing})$, $(\mathsf{category}, \mathsf{nothing})$: she can explain monoids unaided and categories with the book. Upper set: if she can do a task, she can do any easier one. See [[Feasibility Relation]].
