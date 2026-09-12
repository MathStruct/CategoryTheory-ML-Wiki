#exercise #solution #example

**Exercise 6.48.** Eq. (6.47) shows [[Cospan]]s $A \to B$ and $B \to C$ in $\mathbf{Cospan}_{\mathbf{FinSet}}$. Draw their monoidal product as a morphism $A + B \to B + C$.

## Solution

The monoidal product of cospans $A \to N \leftarrow B$ and $B \to P \leftarrow C$ is the cospan $A + B \to N + P \leftarrow B + C$: one simply stacks the two wiring pictures vertically (disjoint union of apices and of feet). See [[Hypergraph Category]] for the general structure.

```tabs
tab: Julia
```julia
using Catlab
c1 = Cospan(FinFunction([1, 1], 2), FinFunction([2], 2))       # A=2 → N=2 ← B=1
c2 = Cospan(FinFunction([1], 3), FinFunction([1, 2, 3], 3))   # B=1 → P=3 ← C=3
c12 = Cospan(oplus(left(c1), left(c2)), oplus(right(c1), right(c2)))
apex(c12)                                                     # FinSet(5) = N + P
```
```

> Sources: 7 Sketches, Exercise 6.48 and Solution A.6.
