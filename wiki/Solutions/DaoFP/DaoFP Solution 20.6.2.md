#solution #proof

**Solution to [[DaoFP Exercise 20.6.2|Exercise 20.6.2]].**

$\mathcal{C}(x, \int_j W j \pitchfork D j) \cong \int_j \mathcal{C}(x, W j \pitchfork D j)$ (continuity) $\cong \int_j \mathbf{Set}(W j, \mathcal{C}(x, D j))$ (power) $\cong [\mathcal{J}, \mathbf{Set}](W, \mathcal{C}(x, D-))$ (natural transformations as an end) $\cong \mathcal{C}(x, \lim^W D)$; conclude by Yoneda. Dually $\mathcal{C}(\int^j W j \cdot D j, x) \cong \int_j \mathbf{Set}(W j, \mathcal{C}(D j, x)) \cong [\mathcal{J}^{\mathrm{op}}, \mathbf{Set}](W, \mathcal{C}(D-, x)) \cong \mathcal{C}(\mathrm{colim}^W D, x)$ using the copower.

> Sources: DaoFP Exercise 20.6.2.
