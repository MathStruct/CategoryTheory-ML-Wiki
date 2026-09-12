#exercise #solution

**Exercise 1.125.** Let $S = \{1,2,3\}$. 1. Pick a preorder $\leq$ on $S$ and write $U(\leq) \subseteq S \times S$. 2. Pick relations $Q \subseteq U(\leq)$ and $Q' \not\subseteq U(\leq)$. 3. Show concretely $\mathrm{Cl}(Q) \sqsubseteq {\leq}$. 4. Show $\mathrm{Cl}(Q') \not\sqsubseteq {\leq}$.

## Solution

1. Take $1 \leq 2 \leq 3$: $U(\leq) = \{(1,1),(2,2),(3,3),(1,2),(2,3),(1,3)\}$.
2. $Q = \{(1,2)\}$, $Q' = \{(2,1)\}$.
3. $\mathrm{Cl}(Q) = \{(1,1),(2,2),(3,3),(1,2)\} \subseteq U(\leq)$.
4. $\mathrm{Cl}(Q') \ni (2,1) \notin U(\leq)$. This illustrates the adjunction $\mathrm{Cl} \dashv U$ of [[Reflexive Transitive Closure]].
