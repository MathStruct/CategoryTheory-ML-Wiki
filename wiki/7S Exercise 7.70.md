#exercise #solution #proof

**Exercise 7.70.** Let $j : \Omega \to \Omega$ satisfy $p \leq j(p)$ for all $p$. Show $j(j(q)) \leq j(q)$ iff $j(j(q)) = j(q)$ ([[Modality]]).

## Solution

($\Leftarrow$) by reflexivity. ($\Rightarrow$) With $p := j(q)$ we have $j(p) \leq p$ by hypothesis and $p \leq j(p)$ by inflation; $\Omega(U)$ is a [[Partial Order|poset]], so $p = j(p)$, i.e. $j(q) = j(j(q))$.

> Sources: 7 Sketches, Exercise 7.70 and Solution A.7.
