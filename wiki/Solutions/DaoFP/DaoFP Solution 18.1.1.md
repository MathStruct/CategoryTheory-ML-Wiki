#solution #proof

**Solution to [[DaoFP Exercise 18.1.1|Exercise 18.1.1]].**

With $a = b = *$: $\int_{F : [\mathcal{M}, \mathbf{Set}]} \mathbf{Set}(F *, F *) \cong \mathcal{M}(*, *)$. By Yoneda, $F * \cong [\mathcal{M}, \mathbf{Set}](\mathcal{M}(*, -), F)$ where $\mathcal{M}(*, -)$ is the *regular representation* — the monoid acting on itself by post-composition. The end becomes $\int_F \mathbf{Set}(\mathrm{Nat}(R, F), \mathrm{Nat}(R, F))$ with $R$ the regular representation, which by the Yoneda corollary in $[\mathcal{M}, \mathbf{Set}]$ is $\mathrm{Nat}(R, R) \cong \mathcal{M}(*, *)$: the equivariant endomaps of the regular representation are exactly right multiplications by monoid elements. So the monoid is recovered from its category of $M$-sets.

> Sources: DaoFP Exercise 18.1.1.
