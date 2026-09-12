#exercise #solution #proof

**Exercise 2.50.** Show the constructions of Theorem 2.49 ([[Preorders are Bool-Categories]]) are mutually inverse.

## Solution

1. From $(P, \leq)$ build $\mathcal{X}_P$ with $\mathcal{X}_P(p, q) = \mathsf{true}$ iff $p \leq q$; the preorder recovered has $p \leq q$ iff $\mathcal{X}_P(p,q) = \mathsf{true}$ iff $p \leq q$ — the original.
2. From a $\mathbf{Bool}$-category $\mathcal{X}$ build the preorder $x \leq y$ iff $\mathcal{X}(x,y) = \mathsf{true}$, then the $\mathbf{Bool}$-category $\mathcal{X}'$ with $\mathcal{X}'(x,y) = \mathsf{true}$ iff $x \leq y$ iff $\mathcal{X}(x,y) = \mathsf{true}$. So $\mathcal{X}' = \mathcal{X}$.
