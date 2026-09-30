#definition #example

Two [[Category|categories]] $\mathcal{C}$ and $\mathcal{D}$ are **equivalent** if there are [[Functor|functors]] $F : \mathcal{C} \to \mathcal{D}$ and $G : \mathcal{D} \to \mathcal{C}$ with [[Natural Isomorphism|natural isomorphisms]] $F \mathbin{;} G \cong \mathrm{id}_{\mathcal{C}}$ and $G \mathbin{;} F \cong \mathrm{id}_{\mathcal{D}}$. Requiring *equalities* instead gives the stricter **isomorphism of categories**, which "involves equality of objects" and is therefore rarely the right notion (DaoFP §10.5). Equivalently, $F$ is fully faithful and essentially surjective.

> Sources: 7 Sketches Remarks 1.74, 2.71, 3.59 ("can be identified with"), Example 3.56; DaoFP §10.5; Kittenlab Lecture 5 (preorders vs. thin categories); CTfS §4.3.4 (Definition 4.3.4.1, Examples 4.3.4.3–4.3.4.7, Proposition 4.3.4.9, Definition 4.3.4.12, Proposition 4.3.4.15), Theorem 4.4.2.3

**Examples.** $\mathbf{Preord} \simeq \mathbf{Bool}\text{-}\mathbf{Cat}$ ([[Preorders are Bool-Categories]]); $\mathbf{Set}^{\underline{1}} \simeq \mathbf{Set}$; $\mathbf{Set}^{\underline{2}} \simeq$ the arrow category of $\mathbf{Set}$; $\mathbf{FinSet}$ is equivalent to its [[Skeleton]] of ordinals $\underline{n}$; any category is equivalent to its skeleton, so a [[Preorder]] is equivalent to its [[Partial Order|poset reflection]]; $\mathcal{C} \times \mathcal{C} \simeq [\mathbf{2}, \mathcal{C}]$. **From Category Theory for Scientists.** Insisting on isomorphism of categories "is akin to saying that two material samples are the same if there is an atom-by-atom matching, or that two words are the same if they are written in the same font, of the same size, by the same person, in the same state of mind" (CTfS §4.3.4). Examples: the indiscrete category on any nonempty set is equivalent to $\mathbf 1$, witnessed by *any* choice of element ([[Codiscrete Category]]); $\mathbf{FLin} \simeq \Delta$, finite linear orders with "funny labels" vs. the standard $[n]$ ([[Simplex Category]]); $\mathbf{Sch} \simeq \mathbf{Cat}$ ([[Categories and Schemas are Equivalent]]); every category is equivalent to the *elected skeleton* obtained by choosing one representative per isomorphism class (CTfS Proposition 4.3.4.9, [[Skeleton]]). A **non-example**: the group $\mathbb{Z}/2$ as a one-object category is *not* equivalent to $\mathbf 1$ even though there are functors both ways and both morphisms of $\mathbb Z/2$ are isomorphisms — the would-be component $\alpha : \star \to \star$ must satisfy $\alpha \circ 1 = 0 \circ \alpha$, impossible since $x + 1 \neq x + 0$ (CTfS Example 4.3.4.7). Equivalences are [[Full and Faithful Functor|fully faithful]] (CTfS Proposition 4.3.4.15), and $\mathbb Z/2 \to \mathbf 1$ is not faithful.

An [[Adjunction]] whose unit and counit are isomorphisms is an equivalence — "a half-equivalence is still very interesting".

````tabs
tab: Lean
```lean
#check CategoryTheory.Equivalence         -- structure: functor, inverse, unitIso, counitIso, functor_unitIso_comp
#check CategoryTheory.Equivalence.ofFullyFaithfullyEssSurj
#check CategoryTheory.currying             -- (C × D ⥤ E) ≌ (C ⥤ D ⥤ E)
```
tab: Haskell
```haskell
-- an equivalence: two functors with natural isomorphisms of the round trips (unenforceable)
data Equiv f g = Equiv (forall a. f a -> g a) (forall a. g a -> f a)   -- for endofunctor-level examples
```
````
