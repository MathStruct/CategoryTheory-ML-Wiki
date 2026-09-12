#exercise #solution #proof

**Exercise 11.4.2.** Let $G = 1$ with $x : 1 \to A$ selecting an element. Using the [[Dependent Product]] adjunction show: (i) $f^* 1$ has singleton fibers over $f^{-1}(x)$ and empty fibers elsewhere; (ii) a map $\phi : f^* 1 \to E$ is a partial section of $E$ over $f^{-1}(x)$; (iii) the fiber of $\Pi_f E$ over $x$ is such a partial section; (iv) what if $A$ is a singleton?

## Solution

(i) $f^* 1 = \{(b, *) \mid f(b) = x\} \cong f^{-1}(x) \subseteq B$, one point over each $b \in f^{-1}(x)$, nothing elsewhere. (ii) A fiberwise map $f^* 1 \to E$ picks, for each $b \in f^{-1}(x)$, an element of $p^{-1}(b)$: a section of $E$ over the patch $f^{-1}(x)$. (iii) The adjunction gives $(\mathcal{C}/B)(f^* 1, E) \cong (\mathcal{C}/A)(\langle 1, x \rangle, \Pi_f E)$, and the right side is the set of points of $\Pi_f E$ lying over $x$; so that fiber *is* the set of partial sections over $f^{-1}(x)$. (iv) If $A = 1$ then $f^{-1}(x) = B$ and $\Pi_f E = S(E)$, the object of global sections.

> Sources: DaoFP Exercise 11.4.2.
