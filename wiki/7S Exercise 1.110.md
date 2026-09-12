#exercise #solution #proof

**Exercise 1.110.** 1. Show that if $f : P \to Q$ has a right adjoint $g$, it is unique up to isomorphism: any other right adjoint $g'$ has $g(q) \cong g'(q)$. 2. Same for left adjoints?

## Solution

1. Using $p \leq g'(f(p))$ with $p = g(q)$ and monotonicity of $g'$ applied to $f(g(q)) \leq q$: $g(q) \leq g'(f(g(q))) \leq g'(q)$. Symmetrically $g'(q) \leq g(q)$.
2. Yes, by the dual argument. (Categorically: adjoints are unique up to unique [[Natural Isomorphism]].)
