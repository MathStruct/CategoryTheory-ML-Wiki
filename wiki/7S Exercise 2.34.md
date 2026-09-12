#exercise #solution

**Exercise 2.34.** On $\mathsf{no} \leq \mathsf{maybe} \leq \mathsf{yes}$ with unit $\mathsf{yes}$ and product $\min$: 1. fill in the table for $\min$; 2. check the axioms for $\mathbf{NMY}$.

## Solution

1. $\min$ takes the smaller element:

| $\min$ | no | maybe | yes |
|---|---|---|---|
| no | no | no | no |
| maybe | no | maybe | maybe |
| yes | no | maybe | yes |

2. (a) $x \leq y, z \leq w \Rightarrow \min(x,z) \leq \min(y,w)$; (b) $\min(x, \mathsf{yes}) = x$; (c), (d) associativity and commutativity of $\min$ — all by checking cases. So $\mathbf{NMY} = (P, \leq, \mathsf{yes}, \min)$ is a [[Symmetric Monoidal Preorder]]. $\mathbf{NMY}$-categories are interpreted in [[7S Exercise 2.61]].
