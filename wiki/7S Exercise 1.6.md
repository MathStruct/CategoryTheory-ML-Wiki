#exercise #solution

**Exercise 1.6.** 1. Write down all partitions of $\{\bullet, \ast\}$, order them, and draw the [[Hasse Diagram]]. 2. Do the same for $\{1,2,3,4\}$ (15 partitions). Choose two systems $A$, $B$. 3. What is $A \vee B$? 4. Is $A \leq A \vee B$ and $B \leq A \vee B$? 5. Which $C$ satisfy $A \leq C$ and $B \leq C$? 6. Is $(A \vee B) \leq C$ in each case?

> 7 Sketches §1.1.2; see [[Preorder of Partitions]], [[Join]].

## Solution

1. $(1)(2) \leq (12)$: two elements, one arrow.
2. The 15 partitions of $\{1,2,3,4\}$ in four rows: bottom $(1)(2)(3)(4)$; then the six with one pair $(12)(3)(4), (13)(2)(4), (14)(2)(3), (1)(23)(4), (1)(24)(3), (1)(2)(34)$; then the seven with a triple or two pairs $(123)(4), (124)(3), (134)(2), (1)(234), (12)(34), (13)(24), (14)(23)$; top $(1234)$.
   Choose $A = (12)(3)(4)$, $B = (13)(2)(4)$.
3. $A \vee B = (123)(4)$.
4. Yes.
5. $C \in \{(123)(4), (1234)\}$.
6. Yes: $(123)(4) \leq (123)(4)$ and $(123)(4) \leq (1234)$.
