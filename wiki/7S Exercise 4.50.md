#exercise #solution

**Exercise 4.50.** In $(\mathbf{Set}, 1, \times)$ with $A = B = C = D = F = G = \mathbb{Z}$, $E = \mathbb{B}$, $f_C(a) = |a|$, $f_D(a) = 5a$, $g_E(d, b) = [d \leq b]$, $g_F(d, b) = d - b$, $h(c, e) = $ if $e$ then $c$ else $1 - c$: compute the listed values and the composite $q : A \times B \to G \times F$.

## Solution

1. $g_E(5,3) = \mathsf{false}$, $g_F(5,3) = 2$. 2. $g_E(3,5) = \mathsf{true}$, $g_F(3,5) = -2$. 3. $h(5, \mathsf{true}) = 5$. 4. $h(-5, \mathsf{true}) = -5$. 5. $h(-5, \mathsf{false}) = 6$. 6. $q_G(-2, 3) = 2$, $q_F(-2,3) = -13$ (since $f_C = 2$, $f_D = -10$, $g_E(-10, 3) = \mathsf{true}$, $g_F = -13$, $h(2, \mathsf{true}) = 2$). 7. $q_G(2,3) = -1$, $q_F(2,3) = 7$ ($f_D = 10$, $g_E(10, 3) = \mathsf{false}$, $h(2, \mathsf{false}) = -1$).
