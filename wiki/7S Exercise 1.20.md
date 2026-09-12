#exercise #solution #proof

**Exercise 1.20.** Let $\sim$ be an [[Equivalence Relation]] on $A$ and $P$ the set of $(\sim)$-closed, $(\sim)$-connected subsets $\{A_p\}$. Show 1. each $A_p$ is nonempty; 2. if $p \neq q$ then $A_p \cap A_q = \varnothing$; 3. $A = \bigcup_p A_p$.

> 7 Sketches Proposition 1.19 ([[Partitions Correspond to Equivalence Relations]]).

## Solution

1. Connected subsets are nonempty by definition.
2. Suppose $a \in A_p \cap A_q$. For $a' \in A_p$, connectedness gives $a \sim a'$, and closedness of $A_q$ gives $a' \in A_q$; symmetrically $A_q \subseteq A_p$. So $A_p = A_q$, contradicting $p \neq q$.
3. For $a \in A$ let $X := \{a' \mid a' \sim a\}$. $X$ is closed (if $a' \in X$ and $b \sim a'$ then $b \sim a$ by transitivity/symmetry), connected (if $b, c \in X$ then $b \sim c$), and contains $a$ (reflexivity). So $a$ lies in some part.
