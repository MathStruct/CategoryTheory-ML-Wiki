#solution #proof

**Solution to [[DaoFP Exercise 17.6.1|Exercise 17.6.1]].**

For an arbitrary set $S$: $\mathbf{Set}(\int^x \mathcal{C}(a, x) \times G x, S) \cong \int_x \mathbf{Set}(\mathcal{C}(a, x) \times G x, S) \cong \int_x \mathbf{Set}(\mathcal{C}(a, x), S^{G x})$. The functor $x \mapsto S^{G x}$ is covariant (contravariant twice), so the covariant ninja Yoneda lemma gives $S^{G a} \cong \mathbf{Set}(G a, S)$. By the Yoneda corollary (objects with isomorphic mapping-outs are isomorphic), $\int^x \mathcal{C}(a, x) \times G x \cong G a$.

> Sources: DaoFP Exercise 17.6.1.
