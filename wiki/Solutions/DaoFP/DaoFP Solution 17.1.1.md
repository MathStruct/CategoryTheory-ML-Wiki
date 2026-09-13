#solution #proof

**Solution to [[DaoFP Exercise 17.1.1|Exercise 17.1.1]].**

Send every object of $\mathcal{C}$ to $0$ and every object of $\mathcal{D}$ to $1$; send morphisms of $\mathcal{C}$ to $\mathrm{id}_0$, morphisms of $\mathcal{D}$ to $\mathrm{id}_1$, and heteromorphisms (elements of $P\langle c, d\rangle$) to the unique arrow $0 \to 1$. Composition is preserved: composing a heteromorphism with a $\mathcal{C}$- or $\mathcal{D}$-morphism is again a heteromorphism, mapped to $0 \to 1 = \mathrm{id}_1 \circ (0 \to 1) = (0 \to 1) \circ \mathrm{id}_0$; there are no composable pairs of heteromorphisms since none go from $\mathcal{D}$ to $\mathcal{C}$.

> Sources: DaoFP Exercise 17.1.1.
