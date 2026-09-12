#exercise #solution #proof

**Exercise 6.35.** Check that the pushout of pushouts from Example 6.33 (three pushouts $Q = X +_A Y$, $R = Y +_B Z$, $S = Q +_Y R$) satisfies the universal property of the [[Colimit]] of the original diagram $X \leftarrow A \to Y \leftarrow B \to Z$.

## Solution

Suppose $T$ is a [[Cocone]] on the original diagram: maps from $X, Y, Z$ to $T$ making the two squares with $A$ and $B$ commute. Since $Q$ is the pushout of $X \leftarrow A \to Y$ there is a unique $Q \to T$ compatible with $X, Y$; since $R$ is the pushout of $Y \leftarrow B \to Z$ there is a unique $R \to T$ compatible with $Y, Z$. Both agree with the given map on $Y$, so $(Q, R, T)$ forms a cocone on $Q \leftarrow Y \to R$, and the pushout $S$ gives a unique $S \to T$ making everything commute. Hence $S$ is the colimit. This is the mechanism behind [[Finite Colimits in Set]]: an initial object and pushouts give all finite colimits.

```tabs
tab: Lean
```lean
import Mathlib
open CategoryTheory Limits
-- finite colimits from an initial object and pushouts
#check @CategoryTheory.Limits.hasFiniteColimits_of_hasInitial_and_pushouts
```
```

> Sources: 7 Sketches, Exercise 6.35 and Solution A.6.
