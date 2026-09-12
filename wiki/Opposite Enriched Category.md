#definition

The **opposite** of a $\mathcal{V}$-[[Enriched Category|category]] $\mathcal{X}$ is the $\mathcal{V}$-category $\mathcal{X}^{\mathrm{op}}$ with $\mathrm{Ob}(\mathcal{X}^{\mathrm{op}}) := \mathrm{Ob}(\mathcal{X})$ and $\mathcal{X}^{\mathrm{op}}(x, y) := \mathcal{X}(y, x)$. Composition uses the symmetry of $\mathcal{V}$ to swap the two hom-objects ([[DaoFP Exercise 20.1.1]]), which is why $\mathcal{V}$ is assumed symmetric.

> Sources: 7 Sketches Exercise 2.73; DaoFP §20.1; cf. [[Opposite Preorder]], [[Opposite Category]].

- A **dagger** $\mathcal{V}$-category is one where the identity function is a [[Enriched Functor|$\mathcal{V}$-functor]] $\dagger : \mathcal{X} \to \mathcal{X}^{\mathrm{op}}$, i.e. $\mathcal{X}(x,y) \leq \mathcal{X}(y,x)$ — symmetric distances for [[Cost]], [[Dagger Preorder|equivalence relations]] for $\mathbf{Bool}$.
- A **skeletal** $\mathcal{V}$-category is one where $I \leq \mathcal{X}(x,y)$ and $I \leq \mathcal{X}(y,x)$ imply $x = y$.
- Skeletal dagger $\mathbf{Cost}$-categories are extended [[Metric Space|metric spaces]]; skeletal dagger $\mathbf{Bool}$-categories are sets ([[7S Exercise 2.73]], [[7S Exercise 1.73]]).
- $\mathcal{V}$-[[Profunctor|profunctors]] are $\mathcal{V}$-functors $\mathcal{X}^{\mathrm{op}} \times \mathcal{Y} \to \mathcal{V}$.
