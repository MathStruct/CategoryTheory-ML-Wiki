#solution #proof

**Solution to [[DaoFP Exercise 20.1.2|Exercise 20.1.2]].**

Given $f : I \to \mathcal{C}(a, b)$ and $g : I \to \mathcal{C}(b, c)$, define $g \circ_0 f := I \cong I \otimes I \xrightarrow{g \otimes f} \mathcal{C}(b, c) \otimes \mathcal{C}(a, b) \xrightarrow{\circ} \mathcal{C}(a, c)$; the identity is $j_a : I \to \mathcal{C}(a, a)$. Associativity follows from the associativity pentagon for $\circ$ together with the coherence of $I \otimes I \cong I$; the unit laws from the unit triangles of $\mathcal{C}$ and the unitors of $\mathcal{V}$. For $\mathcal{V} = \mathbf{Set}$ (with $I = 1$) this recovers $\mathcal{C}$ itself; for $\mathcal{V} = \mathbf{Bool}$ it turns a preorder's truth values into hom-sets of size $0$ or $1$; for a [[Lawvere Metric Space]] the underlying category has an arrow $a \to b$ iff $d(a, b) = 0$.

> Sources: DaoFP Exercise 20.1.2; 7 Sketches §2.3.
