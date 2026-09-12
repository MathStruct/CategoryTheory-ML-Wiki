#exercise #solution #proof

**Exercise 6.24.** For any set $S$ consider the [[Discrete Category]] $\mathbf{Disc}_S$.
1. Show that all [[Pushout]]s exist in $\mathbf{Disc}_S$.
2. For which sets $S$ does $\mathbf{Disc}_S$ have an [[Initial Object]]?

## Solution

1. A span $B \leftarrow A \to C$ in $\mathbf{Disc}_S$ consists of identities, so $A = B = C$, and the square of identities on $A$ is a pushout: any cocone consists of two equal maps $A \to T$ (both identities, so $T = A$), and the identity is the unique mediating map.
2. Exactly when $|S| = 1$. If $S = \varnothing$ there is no object at all; if $s \neq s'$ are two objects there is no morphism $s \to s'$.

> Sources: 7 Sketches, Exercise 6.24 and Solution A.6.
