#solution #proof

**Solution to [[DaoFP Exercise 17.1.2|Exercise 17.1.2]].**

Let $\mathcal{C} = F^{-1}(0)$ and $\mathcal{D} = F^{-1}(1)$ be the full subcategories on the objects sent to $0$ and $1$. Since $\mathbf{2}$ has no arrow $1 \to 0$, there are no morphisms from $\mathcal{D}$-objects to $\mathcal{C}$-objects. Define $P\langle c, d\rangle := \mathcal{E}(c, d)$; it is a [[Profunctor]] $\mathcal{C}^{\mathrm{op}} \times \mathcal{D} \to \mathbf{Set}$ by pre- and post-composition. Then $\mathcal{E}$ is exactly the collage of $P$: objects the disjoint union, hom-sets as in $\mathcal{C}$, $\mathcal{D}$, or $P$, and composition inherited from $\mathcal{E}$.

> Sources: DaoFP Exercise 17.1.2.
