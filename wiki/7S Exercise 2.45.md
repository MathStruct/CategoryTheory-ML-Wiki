#exercise #solution

**Exercise 2.45.** 1. Is $(\mathbb{N}, \leq, 1, \ast)$ a monoidal preorder? 2. Is there a [[Monoidal Monotone Map]] $(\mathbb{N}, \leq, 0, +) \to (\mathbb{N}, \leq, 1, \ast)$? 3. Is $(\mathbb{Z}, \leq, 1, \ast)$ a monoidal preorder?

## Solution

1. Yes ([[7S Exercise 2.31]]). 2. Yes: $f(n) = 1$ for all $n$ (in fact it is the unique one: $f(0) \geq 1$ forces $f(0) = 1$... and $f(n) \cdot f(m) \leq f(n + m)$ with monotonicity pins everything to $1$). 3. No: $\ast$ is not monotone on $\mathbb{Z}$, e.g. $-1 \leq 0$ but $(-1)(-1) = 1 \not\leq 0 = 0 \cdot 0$.
