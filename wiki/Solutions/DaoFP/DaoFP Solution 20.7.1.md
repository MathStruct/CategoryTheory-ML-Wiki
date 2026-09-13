#solution #proof

**Solution to [[DaoFP Exercise 20.7.1|Exercise 20.7.1]].**

Map out to arbitrary $d$: $\mathcal{C}((\mathrm{Lan}_P F) e, d) \cong \mathcal{C}(\int^c \mathcal{B}(P c, e) \cdot F c, d) \cong \int_c \mathcal{C}(\mathcal{B}(P c, e) \cdot F c, d)$ (co-continuity) $\cong \int_c \mathbf{Set}(\mathcal{B}(P c, e), \mathcal{C}(F c, d))$ (copower) $\cong [\mathcal{E}^{\mathrm{op}}, \mathbf{Set}](\mathcal{B}(P-, e), \mathcal{C}(F-, d)) \cong \mathcal{C}(\mathrm{colim}^{\mathcal{B}(P-, e)} F, d)$ by the definition of the weighted colimit. Yoneda finishes the argument; in the enriched setting this formula is taken as the definition.

> Sources: DaoFP Exercise 20.7.1.
