#exercise #solution #proof

**Exercise 3.55.** In the [[Functor Category]] $\mathcal{D}^{\mathcal{C}}$: 1. how do natural transformations compose? 2. what is the identity, and is it unital?

## Solution

1. "For each object $c$, compose the $c$-components": $(\alpha \mathbin{;} \beta)_c := \alpha_c \mathbin{;} \beta_c$. Naturality: the outer rectangle of two pasted naturality squares commutes. "Most beginners think of a natural transformation via its squares, but the main thing is its components; the squares are a check that comes later."
2. $(\mathrm{id}_F)_c := \mathrm{id}_{F(c)}$; its naturality square commutes trivially, and $(\mathrm{id}_F \mathbin{;} \beta)_c = \mathrm{id}_{F(c)} \mathbin{;} \beta_c = \beta_c$.
