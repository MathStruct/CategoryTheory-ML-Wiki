#definition #example

A **cocone** under a [[Diagram]] $D : \mathcal{J} \to \mathcal{C}$ is a [[Cone]] in $\mathcal{C}^{\mathrm{op}}$ (7 Sketches Definition 3.102): an object $x$ with legs $D(j) \to x$ such that all triangles commute; equivalently a [[Natural Transformation]] $D \Rightarrow \Delta_x$. The universal cocone (the [[Initial Object]] in the category of cocones) is the [[Colimit]].

> Sources: 7 Sketches Definition 3.102, §6.2; DaoFP §9.4 ("Cospans as natural transformations", "Functoriality of cospans"), §9.5, §10.7; Kittenlab Lecture 9 ("$\mathrm{Hom}_{\mathcal{C}^{\mathsf{D}}}(F, \Delta(W))$"), 15.

- For the discrete two-object diagram a cocone is a [[Cospan]] $a \to x \leftarrow b$; DaoFP shows the set of cospans over $x$, $[\mathbf{2}, \mathcal{C}](D, \Delta_x)$, is functorial in $x$ (post-compose the legs with $m : x \to y$), and the [[Coproduct]] is the universal cospan.
- For a [[Span]] $X \leftarrow Z \to Y$ a cocone is a commuting square (a [[Pushout]] candidate); for a parallel pair, $q : b \to c$ with $q \circ f = q \circ g$ ([[Coequalizer]]).
- The set of cocones $[\mathcal{J}, \mathcal{C}](D, \Delta_x)$ is itself a [[Limit]] in $\mathbf{Set}$ of $j \mapsto \mathcal{C}(Dj, x)$ — the key step in [[Right Adjoints Preserve Limits]].
- In [[Adjoint Functor Theorem|Freyd's theorem]], the [[Comma Category]] $L \downarrow c$ is a cocone in $\mathcal{C}$ with apex $c$.

````tabs
tab: Lean
```lean
#check CategoryTheory.Limits.Cocone        -- structure Cocone F: pt, ι : F ⟶ (const J).obj pt
#check CategoryTheory.Limits.CoconeMorphism
```
````
