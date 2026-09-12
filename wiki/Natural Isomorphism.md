#definition #theorem

A [[Natural Transformation]] $\alpha : F \Rightarrow G$ is a **natural isomorphism** if every component $\alpha_c$ is an [[Isomorphism]]; equivalently $\alpha$ is an isomorphism in the [[Functor Category]] $[\mathcal{C}, \mathcal{D}]$, and we write $F \cong G$.

> Sources: 7 Sketches Definition 3.49, Remark 3.59; DaoFP §9.2, §9.4, §10.3; Kittenlab Lecture 8, 10, 15.

- **Objects vs. hom-functors** (DaoFP §9.2): $a \cong b$ iff $\mathcal{C}(a, -) \cong \mathcal{C}(b, -)$ iff $\mathcal{C}(-, a) \cong \mathcal{C}(-, b)$ naturally — "either one will do". A [[Representable Functor|representing object]] is unique up to isomorphism (Kittenlab: "that's Yoneda, baby!").
- Universal constructions are natural isomorphisms of hom-functors: $[\mathbf{2}, \mathcal{C}](D, \Delta_x) \cong \mathcal{C}(a + b, x)$ ([[Coproduct]]), $\mathcal{C}(x \times a, b) \cong \mathcal{C}(x, b^a)$ ([[Exponential Object]]), $\mathcal{C}(Lx, y) \cong \mathcal{C}(x, Ry)$ ([[Adjunction]]); naturality encodes the commuting triangles.
- An [[Equivalence of Categories]] is a pair of functors with natural isomorphisms $F \mathbin{;} G \cong \mathrm{id}$ and $G \mathbin{;} F \cong \mathrm{id}$ (7 Sketches Remark 3.59); with equalities instead one has the too-strict *isomorphism of categories* (DaoFP §10.5).
- Kittenlab Lecture 15 proves that composing equivalent [[Cospan|cospans]] gives equivalent results by exhibiting a natural isomorphism of diagrams $F \cong F'$, whence $\mathrm{Hom}(F, \Delta -) \cong \mathrm{Hom}(F', \Delta -)$ and isomorphic [[Pushout|pushouts]].

````tabs
tab: Lean
```lean
#check CategoryTheory.NatIso.ofComponents   -- build F ≅ G from componentwise isos + naturality
#check CategoryTheory.NatIso.isIso_app_of_isIso
```
tab: Haskell
```haskell
-- a natural isomorphism: a pair of natural transformations inverse at every type
data NatIso f g = NatIso (forall a. f a -> g a) (forall a. g a -> f a)
-- e.g. Identity a ≅ (() -> a), (a, b) -> c ≅ a -> b -> c (curry/uncurry)
```
````
