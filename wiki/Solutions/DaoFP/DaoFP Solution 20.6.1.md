#solution #proof

**Solution to [[DaoFP Exercise 20.6.1|Exercise 20.6.1]].**

Map out to an arbitrary $d$: $\mathcal{D}(\mathrm{colim}^{\mathrm{Hom}} P, d) \cong [(\mathcal{C}^{\mathrm{op}} \times \mathcal{C})^{\mathrm{op}}, \mathbf{Set}](\mathcal{C}^{\mathrm{op}}(-, =), \mathcal{D}(P(-, =), d)) \cong \int_{\langle c, c'\rangle} \mathbf{Set}(\mathcal{C}(c', c), \mathcal{D}(P\langle c, c'\rangle, d))$. By Fubini and ninja Yoneda over $c'$ this is $\int_c \mathcal{D}(P\langle c, c\rangle, d) \cong \mathcal{D}(\int^c P\langle c, c\rangle, d)$ (co-continuity of hom). Since $d$ is arbitrary, the Yoneda trick gives $\mathrm{colim}^{\mathrm{Hom}} P \cong \int^c P\langle c, c\rangle$.

> Sources: DaoFP Exercise 20.6.1.
