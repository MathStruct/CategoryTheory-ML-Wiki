#exercise #solution

**Exercise 3.58.** Let $\mathcal{P}$ be a preorder. 1. Is there at most one natural transformation between any two functors $F, G : \mathcal{C} \to \mathcal{P}$? 2. Between $F, G : \mathcal{P} \to \mathcal{C}$?

## Solution

1. True: each component $\alpha_c : F(c) \to G(c)$ lives in a hom-set of $\mathcal{P}$ with at most one element.
2. False: $\mathcal{P} = \underline{1}$, $\mathcal{C} = a \rightrightarrows b$ with $f_1, f_2$, $F(1) = a$, $G(1) = b$: both $\alpha_1 = f_1$ and $\beta_1 = f_2$ are natural transformations. See [[Natural Transformation]].
