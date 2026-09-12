#exercise #solution #proof

**Exercise 4.65.** What are the isomorphisms $X \times \mathbf{1} \cong X$ and $\mathbf{1} \times X \cong X$ in $\mathbf{Prof}_{\mathcal{V}}$?

## Solution

$\alpha : X \times \mathbf{1} \nrightarrow X$, $\alpha((x,1), y) := X(x,y)$, with inverse $\alpha^{-1}(x, (y, 1)) := X(x, y)$. Then $(\alpha^{-1} \mathbin{;} \alpha)(x,z) = \bigvee_y X(x,y) \otimes X(y,z) = X(x,z) = U_X(x,z)$: $\geq$ by $X(x,z) \otimes I \leq X(x,z) \otimes X(z,z)$, $\leq$ by composition in $X$. Similarly $\alpha \mathbin{;} \alpha^{-1} = U_{X \times \mathbf{1}}$, and $\beta((1,x), y) := X(x,y)$ handles $\mathbf{1} \times X$.
