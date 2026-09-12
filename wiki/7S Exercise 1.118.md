#exercise #solution

**Exercise 1.118.** Choose small sets $X, Y$, a function $f : X \to Y$, subsets $B_1, B_2 \subseteq Y$ and $A_1, A_2 \subseteq X$; compute $f^*(B_i)$, $f_!(A_i)$, $f_*(A_i)$.

> See [[Direct Image, Preimage, and Dual Image]].

## Solution

$X = \{a_1, c_1, c_2\}$, $Y = \{a, b, c\}$, $f$ "projects down" ($a_1 \mapsto a$, $c_i \mapsto c$). 1. $f^*\{a, b\} = \{a_1\}$, $f^*\{c\} = \{c_1, c_2\}$. 2. $f_!\varnothing = \varnothing$, $f_!\{a_1, c_1\} = \{a, c\}$. 3. $f_*\varnothing = \{b\}$, $f_*\{a_1, c_1\} = \{a, b\}$.
