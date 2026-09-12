#exercise #solution #annotation

**Exercise 6.78.** The notation $\mathbf{Cospan}_{\mathcal{C}}$ (cospans in $\mathcal{C}$) looks like $\mathbf{Cospan}_F$ ([[Decorated Cospan]]s), though they are different. An expert says "one is a special case of the other: just use the constant functor $F(c) := \{*\}$." What does the expert mean?

## Solution

Take $F : \mathcal{C} \to \mathbf{Set}$ constant at the singleton $\{*\}$; it is lax symmetric monoidal (all structure maps are the unique map into $\{*\}$). A morphism in $\mathbf{Cospan}_F$ is a cospan $X \to N \leftarrow Y$ together with an element of $F(N) = \{*\}$, i.e. no extra choice. Composition is the usual pushout composition since the decoration is forced. Hence $\mathbf{Cospan}_F \cong \mathbf{Cospan}_{\mathcal{C}}$ via the identity-on-objects functor that decorates each cospan with $*$; category theorists happily call these "equal".

> Sources: 7 Sketches, Exercise 6.78 and Solution A.6.
