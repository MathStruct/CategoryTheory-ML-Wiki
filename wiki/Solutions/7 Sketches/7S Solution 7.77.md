#solution #proof

**Solution to [[7S Exercise 7.77|Exercise 7.77]].**

Since $\mathbb{R} = \{[x, x]\}$, $o_{[a,b]} \cap \mathbb{R} = \{x \mid a < x < b\} = B(\tfrac{a+b}{2}, \tfrac{b-a}{2})$. If $U = U' \cap \mathbb{R}$ with $U' = \bigcup_i o_{[a_i, b_i]}$, then $U = \bigcup_i (a_i, b_i)$ is a union of open balls, hence open. Conversely if $U = \bigcup_j B(m_j, \epsilon_j)$, put $a_j = m_j - \epsilon_j$, $b_j = m_j + \epsilon_j$; then $U = \big(\bigcup_j o_{[a_j, b_j]}\big) \cap \mathbb{R}$ is open in the subspace topology.

> Sources: 7 Sketches, Exercise 7.77 and Solution A.7.
