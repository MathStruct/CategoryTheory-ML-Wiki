#definition #example #theorem #proof

A **cospan** in a [[Category]] $\mathcal{C}$ from $X$ to $Y$ is a [[Diagram]] $X \xrightarrow{f} A \xleftarrow{g} Y$ — a [[Cocone]] under the discrete diagram $\{X, Y\}$ (DaoFP: a [[Natural Transformation]] $D \Rightarrow \Delta_A$ for $D : \mathbf{2} \to \mathcal{C}$). The universal cospan is the [[Coproduct]].

> Sources: Kittenlab Lecture 15 ("Cospans": $\mathrm{Csp}(\mathcal{C})$, identity, equivalence, well-definedness); 7 Sketches §6.2.5 (Definition 6.22, Examples 6.23–6.24, Exercises 6.25–6.27: $\mathbf{Cosp}_{\mathcal{C}}$ as a [[Symmetric Monoidal Category]] and [[Hypergraph Category]]), §6.4 ([[Decorated Cospan|decorated cospans]]); DaoFP §9.4 ("Cospans as natural transformations", "Functoriality of cospans").

## The cospan category (Kittenlab Lecture 15)

Let $\mathcal{C}$ have [[Pushout|pushouts]]. **$\mathrm{Csp}(\mathcal{C})$** has objects the objects of $\mathcal{C}$ and morphisms $X \to Y$ the cospans; the composite of $X \to A \leftarrow Y$ and $Y \to B \leftarrow Z$ is $X \to A +_Y B \leftarrow Z$, by pushout. The identity on $X$ is $X \xrightarrow{1_X} X \xleftarrow{1_X} X$.

*The subtlety.* Composing with the identity gives $X +_X A$, which is only *isomorphic* to $A$ — "showing two objects are *equal* is almost always the wrong thing to do, but here objects serve as morphisms, which must be equal on the nose". Two fixes: pass to a [[2-Category|bicategory]] (morphisms between cospans), or — Kittenlab's route — define morphisms as **equivalence classes** of cospans, where $X \to A \leftarrow Y$ and $X \to A' \leftarrow Y$ are **equivalent** if there is an [[Isomorphism]] $\phi : A \to A'$ commuting with the legs.

**Proposition (well-definedness).** Given equivalent cospans $X \to A \leftarrow Y \sim X \to A' \leftarrow Y$ (via $\phi$) and $Y \to B \leftarrow Z \sim Y \to B' \leftarrow Z$ (via $\psi$), there is an isomorphism $\phi +_Y \psi : A +_Y B \to A' +_Y B'$ compatible with all legs. *Proof.* The two pushout spans are functors $F, F' : \mathsf{D} \to \mathcal{C}$ from $\bullet \leftarrow \bullet \to \bullet$; the given isomorphisms $\phi, \psi$ and $1_Y$ assemble into a [[Natural Isomorphism]] $F \cong F'$ (naturality is exactly the commuting of the legs). Then $\mathrm{Hom}(F, \Delta -) \cong \mathrm{Hom}(F', \Delta -)$, and representing objects of isomorphic functors are isomorphic ("that's Yoneda, baby!"). Tracing the construction shows the legs commute. $\blacksquare$ "Our first big serious proof in category theory: when in doubt, go back to definitions."

## Uses

- [[Undirected Wiring Diagram|Undirected wiring diagrams]]: a cospan $X \to A \leftarrow Y$ in $\mathbf{FinSet}$ drawn as boxes with ports joined through junctions $A$ (two styles of picture, Kittenlab Fig. "uwd"). [[Open Graph|Open graphs]]: a graph with input/output maps $I \to G(V) \leftarrow O$ — a cospan of finite sets with a decoration; composing them is gluing along shared vertices.
- $\mathbf{Cosp}_{\mathcal{C}}$ is a [[Hypergraph Category]] (7 Sketches Theorem 6.x), the prototype: every object carries a [[Frobenius Monoid]] given by the cospans $X + X \to X \leftarrow X$ etc.; [[Decorated Cospan|decorated cospans]] and [[Structured Cospan|structured cospans]] add data (circuit components) on the apex. The [[Operad]] of cospans "designs" wiring diagrams (§6.5).
- DaoFP: the set of cospans over $x$ is functorial in $x$, and $[\mathbf{2}, \mathcal{C}](D, \Delta_x) \cong \mathcal{C}(a + b, x)$ defines the sum. Dual: [[Span]].

````tabs
tab: Julia
```julia
using Catlab
# cospans of finite sets; composition by pushout
c1 = Cospan(FinFunction([1, 2], 3), FinFunction([2, 3], 3))     # X=2 → A=3 ← Y=2
c2 = Cospan(FinFunction([1, 1], 2), FinFunction([2], 2))        # Y=2 → B=2 ← Z=1
po = pushout(right(c1), left(c2))                                # glue A and B along Y
c12 = Cospan(compose(left(c1), legs(po)[1]), compose(right(c2), legs(po)[2]))   # X → A +_Y B ← Z
apex(c12)                                                        # FinSet(3)
id_cospan(X) = Cospan(id(X), id(X))
# Catlab computes one specific pushout; the result is well defined up to isomorphism
```
tab: Lean
```lean
#check CategoryTheory.Limits.cospan      -- cospan f g : WalkingCospan ⥤ C
-- Mathlib has spans/cospans as diagrams; the (bi)category of cospans is not built in
```
tab: Haskell
```haskell
data Cospan a x y = Cospan (x -> a) (y -> a)     -- with apex a
-- composition needs pushouts of the apices (see Pushout / Colimit); identity is Cospan id id
idCospan :: Cospan x x x
idCospan = Cospan id id
```
````
