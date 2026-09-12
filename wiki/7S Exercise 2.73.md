#exercise #solution #proof

**Exercise 2.73.** 1. Show that a skeletal dagger [[Cost]]-category is an extended [[Metric Space]]. 2. Make sense of "preorders are to sets as Lawvere metric spaces are to extended metric spaces."

> See [[Opposite Enriched Category]].

## Solution

1. Dagger: the identity is a $\mathbf{Cost}$-functor $\mathcal{X} \to \mathcal{X}^{\mathrm{op}}$, so $d(x,y) \geq d(y,x)$ for all $x,y$, hence by symmetry of the quantifier $d(x,y) = d(y,x)$ — property (c). Skeletal: $0 \geq d(x,y)$ and $0 \geq d(y,x)$ imply $x = y$; given (c) this is property (b). So skeletal dagger $\mathbf{Cost}$-categories are exactly extended metric spaces.
2. By [[7S Exercise 1.73]], skeletal dagger $\mathbf{Bool}$-categories (preorders) are sets. So in both cases "skeletal dagger" turns the enriched notion into the classical one.
