#exercise #solution #example

**Exercise 7.27.** $\mathbb{R}$ with $d(x_1, x_2) = |x_1 - x_2|$ is a [[Metric Space]]. 1. Define the $\epsilon$-ball $B(x, \epsilon)$. 2. When is $U \subseteq \mathbb{R}$ open? 3. Find opens $U_1, U_2$ covering an open $U$. 4. Find an infinite cover.

## Solution

1. $B(x, \epsilon) = \{x' \in \mathbb{R} \mid |x - x'| < \epsilon\} = (x - \epsilon, x + \epsilon)$.
2. $U$ is open iff for every $x \in U$ there is $\epsilon > 0$ with $B(x, \epsilon) \subseteq U$ ([[Topological Space]]).
3. $U_1 = (0, 2)$, $U_2 = (1, 3)$ cover $U = (0, 3)$.
4. $U_i = (\tfrac{1}{i}, 1)$ for $i \in \{1, 2, 3, \dots\}$ cover $U = (0, 1)$.

> Sources: 7 Sketches, Exercise 7.27 and Solution A.7.
