#exercise #solution #proof

**Exercise 5.85.** Show the composite of linear relations $B \subseteq R^m \times R^n$, $C \subseteq R^n \times R^p$ is linear.

## Solution

If $(x, z) \in B \mathbin{;} C$ via $y$, then $(rx, ry) \in B$ and $(ry, rz) \in C$, so $(rx, rz) \in B \mathbin{;} C$; if also $(x', z') \in B \mathbin{;} C$ via $y'$, then $(x + x', y + y') \in B$ and $(y + y', z + z') \in C$, so $(x + x', z + z') \in B \mathbin{;} C$. Hence linear relations form the sub-prop $\mathbf{LinRel}_R$.
