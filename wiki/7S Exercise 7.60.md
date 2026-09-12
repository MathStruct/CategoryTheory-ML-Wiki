#exercise #solution #proof

**Exercise 7.60.** In $\mathbf{Shv}(X)$: 1. which open set is $\mathsf{true}$, given $\mathsf{true} \wedge U = U$ for all $U$? 2. Check $\mathsf{true} \vee U = \mathsf{true}$, $U \Rightarrow \mathsf{true} = \mathsf{true}$, $\mathsf{true} \Rightarrow U = U$. 3. Which open set is $\mathsf{false}$, given $\mathsf{false} \vee U = U$? 4. Check $\mathsf{false} \wedge U = \mathsf{false}$ and $\mathsf{false} \Rightarrow U = \mathsf{true}$.

## Solution

1. Taking $U = X$: $\top \cap X = X$, and $\top \cap X = \top$, so $\top = X$.
2. $X \cup U = X$; $U \Rightarrow X = \bigcup\{R \mid R \cap U \subseteq X\} = X$; $X \Rightarrow U = \bigcup\{R \mid R \subseteq U\} = U$.
3. Taking $U = \varnothing$: $\bot \cup \varnothing = \varnothing$ so $\bot = \varnothing$.
4. $\varnothing \cap U = \varnothing$; $\varnothing \Rightarrow U = \bigcup\{R \mid R \cap \varnothing \subseteq U\} = X$.

> Sources: 7 Sketches, Exercise 7.60 and Solution A.7.
