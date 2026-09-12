#exercise #solution #proof

**Exercise 9.6.2.** Show that $\alpha_x(h) := (F h)(p)$ is natural in $x$.

## Solution

For $f : x \to y$ and $h : a \to x$: $\alpha_y(f \circ h) = F(f \circ h)(p) = F f (F h (p)) = F f(\alpha_x(h))$ by functoriality of $F$; that is $\alpha_y \circ (f \circ -) = Ff \circ \alpha_x$.
