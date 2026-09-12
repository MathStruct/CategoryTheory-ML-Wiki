#exercise #solution

**Exercise 2.62.** Let $\mathcal{M} = (\mathcal{P}(M), \subseteq, M, \cap)$ with $M = \{\mathsf{car}, \mathsf{boat}, \mathsf{foot}\}$ ("modes of transportation"). 1. Draw a graph with four vertices labeled by subsets. 2. Build the $\mathcal{M}$-category (union over paths of the intersection along each path) and write its matrix. 3. Is the interpretation "the hom-object is the set of modes that get you from $a$ to $b$" right?

## Solution

1. $A \xrightarrow{\{\mathsf{boat}\}} B$, $B \xrightarrow{\{\mathsf{boat}\}} D$, $C \xrightarrow{\{\mathsf{foot},\mathsf{boat}\}} A$, $C \xrightarrow{\{\mathsf{foot},\mathsf{car}\}} D$, $D \xrightarrow{\{\mathsf{foot},\mathsf{car}\}} C$ (say).
2. E.g. $\mathcal{X}(C, D)$: paths $C \to A \to B \to D$ (intersection $\{\mathsf{boat}\}$) and $C \to D$ ($\{\mathsf{foot},\mathsf{car}\}$); union $= M$. Diagonal entries are $M$. Taking the union over all paths guarantees $\mathcal{X}(x,y) \cap \mathcal{X}(y,z) \subseteq \mathcal{X}(x,z)$: it is a [[Weighted Graph|presented $\mathcal{V}$-category]].
3. Yes, the interpretation looks right.
