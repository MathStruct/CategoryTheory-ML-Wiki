#exercise #solution #proof

**Exercise 5.20.** Let $(P, \leq_P)$ be the reflexive transitive closure of $R \subseteq P \times P$, $(Q, \leq_Q)$ a preorder, $f : P \to Q$ a function. 1. If $R(x,y) \Rightarrow f(x) \leq f(y)$, show $f$ is monotone. 2. Conversely, if $f$ is monotone then $R(x,y) \Rightarrow f(x) \leq f(y)$.

## Solution

1. $x \leq_P y$ iff there is a chain $x = x_0, \dots, x_n = y$ with $R(x_i, x_{i+1})$ ($n = 0$ gives reflexivity); by assumption $f(x_i) \leq f(x_{i+1})$, so by induction and transitivity $f(x) \leq f(y)$.
2. $R(x,y)$ implies $x \leq_P y$ (the closure contains $R$), hence $f(x) \leq f(y)$.
This is the universal property of the [[Reflexive Transitive Closure|free preorder]] — about maps *out* ([[Free Prop]]).
