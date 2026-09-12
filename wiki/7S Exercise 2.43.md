#exercise #solution

**Exercise 2.43.** Check that $g : \mathbf{Bool} \to \mathbf{Cost}$, $g(\mathsf{false}) = \infty$, $g(\mathsf{true}) = 0$, is a [[Monoidal Monotone Map]]; is it strict?

## Solution

Monotone: $\mathsf{false} \leq \mathsf{true}$ and $\infty \geq 0$. (a): $0 \geq 0 = g(\mathsf{true})$. (b): $g(a) + g(b) \geq g(a \wedge b)$ in all four cases ($\infty + \infty \geq \infty$, $\infty + 0 \geq \infty$, $0 + 0 \geq 0$). All are equalities, so $g$ is strict.
