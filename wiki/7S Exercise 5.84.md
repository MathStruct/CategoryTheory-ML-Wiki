#exercise #solution #proof

**Exercise 5.84.** Over a field $R$: 1. composing $g$ with reversed zeros gives $\ker S(g)$; 2. composing reversed discards with $g$ gives $\mathrm{im}\, S(g)$; 3. $B(g)$ is a linear subspace.

## Solution

1. The reversed zero has behaviour $\{y \mid y = 0\}$; its $n$-fold sum is $\{0\} \subseteq R^n$; composing with $B(g)$ gives $\{x \mid S(g)x = 0\}$. 2. Reversed discard has behaviour all of $R$; composing $R^m$ with $B(g)$ gives $\{y \mid \exists x.\ S(g)x = y\}$. 3. $S(g)$ is linear, so $B(g)$ is closed under scalars and sums; likewise $B(g^{\mathrm{op}})$; and composites of linear relations are linear ([[7S Exercise 5.85]]). See [[Graphical Linear Algebra]].
