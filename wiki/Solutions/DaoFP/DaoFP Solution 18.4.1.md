#solution #annotation

**Solution to [[DaoFP Exercise 18.4.1|Exercise 18.4.1]].**

If $\mathcal{D} = \mathbf{1}$, the second hom-set $\mathcal{D}(m \bullet b, t)$ is a singleton and the optic reduces to $\int^{m} \mathcal{C}(s, m \times a)$; by co-Yoneda-style reasoning this is just $\mathcal{C}(s, a)$ up to the residue — a *getter* (the $b, t$ side is trivial). With the first category $\mathcal{C}^{\mathrm{op}} \times \mathcal{C}$ and the second terminal, $s$ and $a$ are pairs and the optic is $\int^{m} (\mathcal{C}^{\mathrm{op}} \times \mathcal{C})(\langle s, t\rangle, m \bullet \langle a, b\rangle) = \int^m \mathcal{C}(m \times a, s) \times \mathcal{C}(t, m \times b)$ — the existential lens with all arrows reversed, i.e. a lens in the opposite category.

> Sources: DaoFP Exercise 18.4.1.
