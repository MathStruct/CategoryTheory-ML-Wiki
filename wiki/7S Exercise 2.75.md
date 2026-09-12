#exercise #solution #proof

**Exercise 2.75.** Verify that the [[Product of Enriched Categories|$\mathcal{V}$-product]] $\mathcal{X} \times \mathcal{Y}$ is a $\mathcal{V}$-category, and point out where symmetry is used.

## Solution

1. $I = I \otimes I \leq \mathcal{X}(x,x) \otimes \mathcal{Y}(y,y) = (\mathcal{X} \times \mathcal{Y})((x,y),(x,y))$.
2. $\mathcal{X}(x_1,x_2) \otimes \mathcal{Y}(y_1,y_2) \otimes \mathcal{X}(x_2,x_3) \otimes \mathcal{Y}(y_2,y_3) \cong \mathcal{X}(x_1,x_2) \otimes \mathcal{X}(x_2,x_3) \otimes \mathcal{Y}(y_1,y_2) \otimes \mathcal{Y}(y_2,y_3) \leq \mathcal{X}(x_1,x_3) \otimes \mathcal{Y}(y_1,y_3)$ by monotonicity.
3. Symmetry is used to swap $\mathcal{Y}(y_1,y_2) \otimes \mathcal{X}(x_2,x_3) \cong \mathcal{X}(x_2,x_3) \otimes \mathcal{Y}(y_1,y_2)$.
