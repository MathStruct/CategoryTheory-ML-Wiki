#exercise #solution #example

**Exercise 7.44.** With $U_1 = \{a, b\}$, $U_2 = \{b, e\}$, overlap $\{b\}$, in the [[Sheaf of Sections]] $\mathrm{Sec}_f$: 1. find $s_1 \in \mathrm{Sec}_f(U_1)$, $s_2 \in \mathrm{Sec}_f(U_2)$ not agreeing on the overlap; 2. can they be glued? 3. find $h_1, h_2$ that agree but differ from Eq. (7.43); 4. can they be glued?

## Solution

1. $s_1 = (a_1, b_1)$, $s_2 = (b_2, e_1)$.
2. No: a section over $\{a, b, e\}$ has a single $b$-value, which would have to be both $b_1$ and $b_2$.
3. $h_1 = (a_2, b_3)$, $h_2 = (b_3, e_2)$.
4. Yes, uniquely: $h = (a_2, b_3, e_2)$. This is the [[Sheaf|sheaf condition]] in action.

> Sources: 7 Sketches, Exercise 7.44 and Solution A.7.
