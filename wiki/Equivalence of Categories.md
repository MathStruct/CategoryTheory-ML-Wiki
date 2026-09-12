#definition #example

Two [[Category|categories]] $\mathcal{C}$ and $\mathcal{D}$ are **equivalent** if there are [[Functor|functors]] $F : \mathcal{C} \to \mathcal{D}$ and $G : \mathcal{D} \to \mathcal{C}$ with [[Natural Isomorphism|natural isomorphisms]] $F \mathbin{;} G \cong \mathrm{id}_{\mathcal{C}}$ and $G \mathbin{;} F \cong \mathrm{id}_{\mathcal{D}}$. Requiring *equalities* instead gives the stricter **isomorphism of categories**, which "involves equality of objects" and is therefore rarely the right notion (DaoFP §10.5). Equivalently, $F$ is fully faithful and essentially surjective.

> Sources: 7 Sketches Remarks 1.74, 2.71, 3.59 ("can be identified with"), Example 3.56; DaoFP §10.5; Kittenlab Lecture 5 (preorders vs. thin categories).

**Examples.** $\mathbf{Preord} \simeq \mathbf{Bool}\text{-}\mathbf{Cat}$ ([[Preorders are Bool-Categories]]); $\mathbf{Set}^{\underline{1}} \simeq \mathbf{Set}$; $\mathbf{Set}^{\underline{2}} \simeq$ the arrow category of $\mathbf{Set}$; $\mathbf{FinSet}$ is equivalent to its [[Skeleton]] of ordinals $\underline{n}$; any category is equivalent to its skeleton, so a [[Preorder]] is equivalent to its [[Partial Order|poset reflection]]; $\mathcal{C} \times \mathcal{C} \simeq [\mathbf{2}, \mathcal{C}]$. An [[Adjunction]] whose unit and counit are isomorphisms is an equivalence — "a half-equivalence is still very interesting".

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
