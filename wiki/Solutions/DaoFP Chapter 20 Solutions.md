#solution

Solutions to the exercises of DaoFP, Chapter 20: [[DaoFP Chapter 20 Exercises]]. Index: [[Map of Content]].

## Solution 20.1.1

#proof — [[DaoFP Chapter 20 Exercises#Exercise 20.1.1|Exercise 20.1.1]]

$\mathcal{C}^{\mathrm{op}}(a, b) := \mathcal{C}(b, a)$. Composition $\mathcal{C}^{\mathrm{op}}(b, c) \otimes \mathcal{C}^{\mathrm{op}}(a, b) \to \mathcal{C}^{\mathrm{op}}(a, c)$ is $\mathcal{C}(c, b) \otimes \mathcal{C}(b, a) \xrightarrow{\gamma} \mathcal{C}(b, a) \otimes \mathcal{C}(c, b) \xrightarrow{\circ_\mathcal{C}} \mathcal{C}(c, a)$ — the symmetry $\gamma$ of $\mathcal{V}$ swaps the factors so that $\mathcal{C}$'s composition applies; the unit $j_a : I \to \mathcal{C}^{\mathrm{op}}(a, a) = \mathcal{C}(a, a)$ is $\mathcal{C}$'s unit. Associativity and unit laws follow from those of $\mathcal{C}$ and the coherence of $\gamma$; this is why $\mathcal{V}$ must be symmetric to form opposites.

> Sources: DaoFP Exercise 20.1.1; 7 Sketches Exercise 2.63.

## Solution 20.1.2

#proof — [[DaoFP Chapter 20 Exercises#Exercise 20.1.2|Exercise 20.1.2]]

Given $f : I \to \mathcal{C}(a, b)$ and $g : I \to \mathcal{C}(b, c)$, define $g \circ_0 f := I \cong I \otimes I \xrightarrow{g \otimes f} \mathcal{C}(b, c) \otimes \mathcal{C}(a, b) \xrightarrow{\circ} \mathcal{C}(a, c)$; the identity is $j_a : I \to \mathcal{C}(a, a)$. Associativity follows from the associativity pentagon for $\circ$ together with the coherence of $I \otimes I \cong I$; the unit laws from the unit triangles of $\mathcal{C}$ and the unitors of $\mathcal{V}$. For $\mathcal{V} = \mathbf{Set}$ (with $I = 1$) this recovers $\mathcal{C}$ itself; for $\mathcal{V} = \mathbf{Bool}$ it turns a preorder's truth values into hom-sets of size $0$ or $1$; for a [[Lawvere Metric Space]] the underlying category has an arrow $a \to b$ iff $d(a, b) = 0$.

> Sources: DaoFP Exercise 20.1.2; 7 Sketches §2.3.

## Solution 20.2.1

#example — [[DaoFP Chapter 20 Exercises#Exercise 20.2.1|Exercise 20.2.1]]

A $\mathbf{Bool}$-functor $F : P \to Q$ is a function on objects with $P(a, b) \leq Q(F a, F b)$ in $\mathbf{Bool}$, i.e. $a \leq b \Rightarrow F a \leq F b$ — a [[Monotone Map]] (7 Sketches Proposition 2.x: $\mathbf{Bool}$-functors are monotone maps, [[Cost]]-functors are Lipschitz maps).

> Sources: DaoFP Exercise 20.2.1; 7 Sketches §2.4.2.

## Solution 20.2.2

#proof — [[DaoFP Chapter 20 Exercises#Exercise 20.2.2|Exercise 20.2.2]]

Its action on internal homs must be a map $[a, a'] \otimes [b, b'] \to [a \otimes b, a' \otimes b']$. By the currying adjunction such a map corresponds to $[a, a'] \otimes [b, b'] \otimes a \otimes b \to a' \otimes b'$; rearrange with the symmetry to $([a, a'] \otimes a) \otimes ([b, b'] \otimes b)$ and apply the evaluation counits $\varepsilon \otimes \varepsilon$. Preservation of composition and identities follows from the corresponding properties of evaluation.

> Sources: DaoFP Exercise 20.2.2.

## Solution 20.6.1

#proof — [[DaoFP Chapter 20 Exercises#Exercise 20.6.1|Exercise 20.6.1]]

Map out to an arbitrary $d$: $\mathcal{D}(\mathrm{colim}^{\mathrm{Hom}} P, d) \cong [(\mathcal{C}^{\mathrm{op}} \times \mathcal{C})^{\mathrm{op}}, \mathbf{Set}](\mathcal{C}^{\mathrm{op}}(-, =), \mathcal{D}(P(-, =), d)) \cong \int_{\langle c, c'\rangle} \mathbf{Set}(\mathcal{C}(c', c), \mathcal{D}(P\langle c, c'\rangle, d))$. By Fubini and ninja Yoneda over $c'$ this is $\int_c \mathcal{D}(P\langle c, c\rangle, d) \cong \mathcal{D}(\int^c P\langle c, c\rangle, d)$ (co-continuity of hom). Since $d$ is arbitrary, the Yoneda trick gives $\mathrm{colim}^{\mathrm{Hom}} P \cong \int^c P\langle c, c\rangle$.

> Sources: DaoFP Exercise 20.6.1.

## Solution 20.6.2

#proof — [[DaoFP Chapter 20 Exercises#Exercise 20.6.2|Exercise 20.6.2]]

$\mathcal{C}(x, \int_j W j \pitchfork D j) \cong \int_j \mathcal{C}(x, W j \pitchfork D j)$ (continuity) $\cong \int_j \mathbf{Set}(W j, \mathcal{C}(x, D j))$ (power) $\cong [\mathcal{J}, \mathbf{Set}](W, \mathcal{C}(x, D-))$ (natural transformations as an end) $\cong \mathcal{C}(x, \lim^W D)$; conclude by Yoneda. Dually $\mathcal{C}(\int^j W j \cdot D j, x) \cong \int_j \mathbf{Set}(W j, \mathcal{C}(D j, x)) \cong [\mathcal{J}^{\mathrm{op}}, \mathbf{Set}](W, \mathcal{C}(D-, x)) \cong \mathcal{C}(\mathrm{colim}^W D, x)$ using the copower.

> Sources: DaoFP Exercise 20.6.2.

## Solution 20.7.1

#proof — [[DaoFP Chapter 20 Exercises#Exercise 20.7.1|Exercise 20.7.1]]

Map out to arbitrary $d$: $\mathcal{C}((\mathrm{Lan}_P F) e, d) \cong \mathcal{C}(\int^c \mathcal{B}(P c, e) \cdot F c, d) \cong \int_c \mathcal{C}(\mathcal{B}(P c, e) \cdot F c, d)$ (co-continuity) $\cong \int_c \mathbf{Set}(\mathcal{B}(P c, e), \mathcal{C}(F c, d))$ (copower) $\cong [\mathcal{E}^{\mathrm{op}}, \mathbf{Set}](\mathcal{B}(P-, e), \mathcal{C}(F-, d)) \cong \mathcal{C}(\mathrm{colim}^{\mathcal{B}(P-, e)} F, d)$ by the definition of the weighted colimit. Yoneda finishes the argument; in the enriched setting this formula is taken as the definition.

> Sources: DaoFP Exercise 20.7.1.
