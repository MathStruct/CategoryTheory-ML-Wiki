#exercise #solution #proof

**Exercise 4.66.** Check the snake equations for $\eta_X(1, x, x') = X(x,x')$ and $\varepsilon_X(x, x', 1) = X(x, x')$ in $\mathbf{Prof}_{\mathcal{V}}$.

## Solution

The composite $X \xrightarrow{\alpha^{-1}} X \times \mathbf{1} \xrightarrow{U_X \times \eta_X} X \times X^{\mathrm{op}} \times X \xrightarrow{\varepsilon_X \times U_X} \mathbf{1} \times X \xrightarrow{\alpha} X$ has value at $(x, y)$ equal to $\bigvee_{a,b,c,d,e} X(x,a) \otimes X(a,b) \otimes X(c,d) \otimes X(b,c) \otimes X(d,e) \otimes X(e,y)$ (using distributivity), which collapses to $X(x,y)$ by repeatedly applying Lemma 4.27 (composing with the unit profunctor is the identity). So the composite is $U_X$; the other snake equation is analogous. See [[Compact Closed Category]].
