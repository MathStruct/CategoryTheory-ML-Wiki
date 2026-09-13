#solution

**Solution to [[7S Exercise 2.62|Exercise 2.62]].**

1. $A \xrightarrow{\{\mathsf{boat}\}} B$, $B \xrightarrow{\{\mathsf{boat}\}} D$, $C \xrightarrow{\{\mathsf{foot},\mathsf{boat}\}} A$, $C \xrightarrow{\{\mathsf{foot},\mathsf{car}\}} D$, $D \xrightarrow{\{\mathsf{foot},\mathsf{car}\}} C$ (say).
2. E.g. $\mathcal{X}(C, D)$: paths $C \to A \to B \to D$ (intersection $\{\mathsf{boat}\}$) and $C \to D$ ($\{\mathsf{foot},\mathsf{car}\}$); union $= M$. Diagonal entries are $M$. Taking the union over all paths guarantees $\mathcal{X}(x,y) \cap \mathcal{X}(y,z) \subseteq \mathcal{X}(x,z)$: it is a [[Weighted Graph|presented $\mathcal{V}$-category]].
3. Yes, the interpretation looks right.

> Sources: 7 Sketches, Exercise 2.62 and Solution A.2.
