#solution #proof

**Solution to [[DaoFP Exercise 20.2.2|Exercise 20.2.2]].**

Its action on internal homs must be a map $[a, a'] \otimes [b, b'] \to [a \otimes b, a' \otimes b']$. By the currying adjunction such a map corresponds to $[a, a'] \otimes [b, b'] \otimes a \otimes b \to a' \otimes b'$; rearrange with the symmetry to $([a, a'] \otimes a) \otimes ([b, b'] \otimes b)$ and apply the evaluation counits $\varepsilon \otimes \varepsilon$. Preservation of composition and identities follows from the corresponding properties of evaluation.

> Sources: DaoFP Exercise 20.2.2.
