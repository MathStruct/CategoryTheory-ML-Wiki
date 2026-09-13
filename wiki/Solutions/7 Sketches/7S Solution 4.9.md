#solution #proof

**Solution to [[7S Exercise 4.9|Exercise 4.9]].**

A $\mathcal{V}$-functor condition reads $(\mathcal{X}^{\mathrm{op}} \times \mathcal{Y})((x,y),(x',y')) \leq \mathcal{V}(\Phi(x,y), \Phi(x',y'))$, i.e. $\mathcal{X}(x', x) \otimes \mathcal{Y}(y, y') \leq \Phi(x, y) \multimap \Phi(x', y')$ using the [[Opposite Enriched Category|opposite]], the [[Product of Enriched Categories|product]] and self-enrichment. By the hom-element adjunction (2.80) and symmetry this is $\mathcal{X}(x',x) \otimes \Phi(x,y) \otimes \mathcal{Y}(y,y') \leq \Phi(x',y')$.

> Sources: 7 Sketches, Exercise 4.9 and Solution A.4.
