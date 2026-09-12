#exercise #solution #proof

**Exercise 1.98.** Find a right adjoint for $(3 \times -) : \mathbb{Z} \to \mathbb{R}$ and show it is correct.

> See [[Galois Connection]], Example 1.97.

## Solution

The right adjoint is $\lfloor -/3 \rfloor : \mathbb{R} \to \mathbb{Z}$; we must show $3z \leq r$ iff $z \leq \lfloor r/3 \rfloor$. If $z \leq \lfloor r/3 \rfloor$ then $3z \leq 3 \lfloor r/3 \rfloor \leq r$. If $3z \leq r$ then $z \leq r/3$, and since $z$ is an integer below $r/3$ it is below the greatest such, $\lfloor r/3 \rfloor$.
