#exercise #solution #proof

**Exercise 7.80.** Fix a [[Topological Space]] $X$ and a subset $R \subseteq \mathbb{I}\mathbb{R}$; define $H_X(U) := \{f : U \cap R \to X \text{ continuous}\}$. 1. Is $H_X$ a [[Presheaf]]? What are the restriction maps? 2. Is it a [[Sheaf]]?

## Solution

1. Yes: for $V \subseteq U$ restrict $f$ along $V \cap R \subseteq U \cap R$; this is functorial.
2. Yes: given a cover $U = \bigcup_i U_i$ and continuous $f_i : U_i \cap R \to X$ agreeing on overlaps, they glue to a unique continuous function on $U \cap R = \bigcup_i (U_i \cap R)$ (continuity is local). So $H_X \in \mathbf{Shv}(\mathbb{I}\mathbb{R})$ — a [[Topos of Behavior Types|behavior type]]; with $R = \mathbb{R}$ this is $G_X$ of Example 7.79.

> Sources: 7 Sketches, Exercise 7.80 and Solution A.7.
