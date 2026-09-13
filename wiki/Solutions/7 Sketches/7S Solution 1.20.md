#solution #proof

**Solution to [[7S Exercise 1.20|Exercise 1.20]].**

1. Connected subsets are nonempty by definition.
2. Suppose $a \in A_p \cap A_q$. For $a' \in A_p$, connectedness gives $a \sim a'$, and closedness of $A_q$ gives $a' \in A_q$; symmetrically $A_q \subseteq A_p$. So $A_p = A_q$, contradicting $p \neq q$.
3. For $a \in A$ let $X := \{a' \mid a' \sim a\}$. $X$ is closed (if $a' \in X$ and $b \sim a'$ then $b \sim a$ by transitivity/symmetry), connected (if $b, c \in X$ then $b \sim c$), and contains $a$ (reflexivity). So $a$ lies in some part.

> Sources: 7 Sketches, Exercise 1.20 and Solution A.1.
