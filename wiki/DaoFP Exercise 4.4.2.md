#exercise #solution #proof

**Exercise 4.4.2.** Show that the bijection $\beta_x : \mathcal{C}(a + b, x) \to \mathcal{C}(b + a, x)$, $h = [f, g] \mapsto h' = [g, f]$, is natural ([[Sum Type]], [[Natural Transformation]]).

## Solution

Change focus along $k : x \to y$. Post-composing $h = [f, g]$ with $k$ gives $k \circ h = [k \circ f, k \circ g]$ (by uniqueness of copairing); applying $\beta_y$ yields $[k \circ g, k \circ f]$. Alternatively apply $\beta_x$ first, getting $[g, f]$, then post-compose: $k \circ [g, f] = [k \circ g, k \circ f]$. Both routes agree, so $\beta$ is natural; by the [[Yoneda Lemma]] $a + b \cong b + a$.

> Sources: DaoFP Exercise 4.4.2.
