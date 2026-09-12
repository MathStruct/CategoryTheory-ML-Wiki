#exercise #solution #proof

**Exercise 4.9.** Show that a $\mathcal{V}$-[[Profunctor]] $\Phi : \mathcal{X}^{\mathrm{op}} \times \mathcal{Y} \to \mathcal{V}$ is the same as a function $\Phi : \mathrm{Ob}(\mathcal{X}) \times \mathrm{Ob}(\mathcal{Y}) \to V$ with $\mathcal{X}(x', x) \otimes \Phi(x, y) \otimes \mathcal{Y}(y, y') \leq \Phi(x', y')$.

## Solution

A $\mathcal{V}$-functor condition reads $(\mathcal{X}^{\mathrm{op}} \times \mathcal{Y})((x,y),(x',y')) \leq \mathcal{V}(\Phi(x,y), \Phi(x',y'))$, i.e. $\mathcal{X}(x', x) \otimes \mathcal{Y}(y, y') \leq \Phi(x, y) \multimap \Phi(x', y')$ using the [[Opposite Enriched Category|opposite]], the [[Product of Enriched Categories|product]] and self-enrichment. By the hom-element adjunction (2.80) and symmetry this is $\mathcal{X}(x',x) \otimes \Phi(x,y) \otimes \mathcal{Y}(y,y') \leq \Phi(x',y')$.
