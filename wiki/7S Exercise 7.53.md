#exercise #solution #proof

**Exercise 7.53.** 1. Show that $\Omega(U) = \{U' \subseteq U \text{ open}\}$ with restriction $U' \mapsto U' \cap V$ is functorial. 2. Is that all that is needed for $\Omega$ to be a [[Presheaf]]?

## Solution

1. For $W \subseteq V \subseteq U$: $(U' \cap V) \cap W = U' \cap W$ since $W \subseteq V$; identities: $U' \cap U = U'$ for $U' \subseteq U$.
2. Yes: a presheaf is just a functor $\mathrm{Op}^{\mathrm{op}} \to \mathbf{Set}$, and functoriality is all there is to check. (That it is moreover a [[Sheaf]] — the [[Subobject Classifier]] of $\mathbf{Shv}(X)$ — is verified separately.)

> Sources: 7 Sketches, Exercise 7.53 and Solution A.7.
