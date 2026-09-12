#exercise #solution #annotation

**Exercise 7.34.** If $(X, \mathrm{Op})$ is a [[Topological Space]] and $\mathcal{V} = (\mathrm{Op}, \subseteq, X, \cap)$ the corresponding [[Quantale]] (Remark 7.33), how might we imagine a $\mathcal{V}$-[[Enriched Category|category]]?

## Solution

A $\mathcal{V}$-category has objects and, for each pair $a, b$, an open set $\mathcal{C}(a, b) \subseteq X$, with $X \subseteq \mathcal{C}(a, a)$ and $\mathcal{C}(a, b) \cap \mathcal{C}(b, c) \subseteq \mathcal{C}(a, c)$. Think of $\mathcal{C}(a, b)$ as a *size restriction* for getting from $a$ to $b$ — bridges your truck must fit under. Going from $a$ to itself has no restriction ($X$). Along a path you must fit under every bridge (meet $= \cap$), and you may take any path (join $= \cup$): as in [[Matrix Multiplication in a Quantale]], $\mathcal{C}(B, C) = (U_3 \cap U_1) \cup (U_4 \cap U_2)$ for the two paths of the book's example.

> Sources: 7 Sketches, Exercise 7.34 and Solution A.7.
