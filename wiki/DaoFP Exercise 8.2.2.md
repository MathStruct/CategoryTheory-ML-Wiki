#exercise #solution

**Exercise 8.2.2.** Show a functor from the "walking iso" ($a \rightleftarrows b$ with $g \circ f = \mathrm{id}_a$, $f \circ g = \mathrm{id}_b$) picks an isomorphism.

## Solution

$F(g) \circ F(f) = F(g \circ f) = F(\mathrm{id}_a) = \mathrm{id}_{F a}$ and likewise $F(f) \circ F(g) = \mathrm{id}_{Fb}$, so $F(f)$ is an [[Isomorphism]] with inverse $F(g)$.
