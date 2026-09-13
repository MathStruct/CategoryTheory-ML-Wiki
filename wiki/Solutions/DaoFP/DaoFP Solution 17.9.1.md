#solution #proof

**Solution to [[DaoFP Exercise 17.9.1|Exercise 17.9.1]].**

Given $f : x' \to x$ and $g : y \to y'$, map $(l, r) \mapsto ((g \times \mathrm{id}_a) \circ l,\ r \circ (f \times \mathrm{id}_b))$: the first factor is covariant in $y$ (post-composition), the second contravariant in $x$ (pre-composition). Functoriality follows from that of composition and of $\times$. So it is a functor $\mathcal{C}^{\mathrm{op}} \times \mathcal{C} \to \mathbf{Set}$ in $\langle x, y\rangle$ with $\langle s, t\rangle, \langle a, b\rangle$ fixed, and its coend is the existential lens.

> Sources: DaoFP Exercise 17.9.1.
