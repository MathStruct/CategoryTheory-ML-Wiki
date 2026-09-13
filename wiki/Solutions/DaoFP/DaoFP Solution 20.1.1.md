#solution #proof

**Solution to [[DaoFP Exercise 20.1.1|Exercise 20.1.1]].**

$\mathcal{C}^{\mathrm{op}}(a, b) := \mathcal{C}(b, a)$. Composition $\mathcal{C}^{\mathrm{op}}(b, c) \otimes \mathcal{C}^{\mathrm{op}}(a, b) \to \mathcal{C}^{\mathrm{op}}(a, c)$ is $\mathcal{C}(c, b) \otimes \mathcal{C}(b, a) \xrightarrow{\gamma} \mathcal{C}(b, a) \otimes \mathcal{C}(c, b) \xrightarrow{\circ_\mathcal{C}} \mathcal{C}(c, a)$ — the symmetry $\gamma$ of $\mathcal{V}$ swaps the factors so that $\mathcal{C}$'s composition applies; the unit $j_a : I \to \mathcal{C}^{\mathrm{op}}(a, a) = \mathcal{C}(a, a)$ is $\mathcal{C}$'s unit. Associativity and unit laws follow from those of $\mathcal{C}$ and the coherence of $\gamma$; this is why $\mathcal{V}$ must be symmetric to form opposites.

> Sources: DaoFP Exercise 20.1.1; 7 Sketches Exercise 2.63.
