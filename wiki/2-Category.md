#definition #example

A **2-category** has objects (**0-cells**), arrows between objects (**1-cells**) and arrows between arrows (**2-cells**), with two compositions of 2-cells (vertical and horizontal) satisfying an interchange law. A 2-category whose laws hold "on the nose" is **strict**; weakening them to isomorphisms gives a *bicategory*.

> Sources: DaoFP §9.9 ("2-category $\mathbf{Cat}$"), §10.5, §10.10, §17.8 (the bicategory of profunctors), §20.1 (2-categories are $\mathbf{Cat}$-enriched); Kittenlab Lecture 7 ("morphisms that go between morphisms are called 2-morphisms"; Merry and Pippin miss higher category theory), 15 (bicategories of cospans); 7 Sketches §4.3 (V-Prof as a category "up to isomorphism").

- **$\mathbf{Cat}$** is a strict 2-category: 0-cells are categories, 1-cells [[Functor|functors]], 2-cells [[Natural Transformation|natural transformations]]; each hom-set is a [[Functor Category]]. Equivalently $\mathbf{Cat}$ is [[Enriched Category|enriched]] in $(\mathbf{Cat}, \times, \mathbf{1})$ — "the 2-category of small categories is enriched in itself".
- [[Adjunction|Adjunctions]] (via [[Unit and Counit of an Adjunction|unit/counit]] and triangle identities) and [[Monad|monads]] can be defined in any 2-category; the category of adjunctions $\mathbf{Adj}(\mathbf{Cat})$ is itself a 2-category (DaoFP §10.10); monads in the bicategory $\mathbf{Prof}$ are [[Prearrow|prearrows]] (§17.8).
- [[Cospan|Cospans]] and [[Profunctor|profunctors]] naturally form bicategories (composition is only associative up to iso); Kittenlab instead takes isomorphism classes to get an honest category $\mathrm{Csp}(\mathcal{C})$, "not walking that route today".
- $n$-categories have cells up to level $n$; $\infty$-categories have cells all the way up and are used in algebraic topology (points, paths, surfaces swept by paths, …).
- [[String Diagram|String diagrams]] (DaoFP §15.1) are the graphical calculus for 2-categories: regions are 0-cells, strings 1-cells, dots 2-cells.

````tabs
tab: Lean
```lean
#check CategoryTheory.Bicategory        -- bicategories (weak 2-categories)
#check CategoryTheory.Bicategory.Strict
#check CategoryTheory.Cat.bicategory    -- Cat as a bicategory (in fact strict)
```
tab: Haskell
```haskell
-- 2-cells between endofunctors as polymorphic functions; horizontal and vertical composition
type Nat f g = forall a. f a -> g a
vert :: Nat g h -> Nat f g -> Nat f h
vert beta alpha = beta . alpha
horiz :: Functor g => Nat g g' -> Nat f f' -> (forall a. g (f a) -> g' (f' a))
horiz beta alpha = beta . fmap alpha
```
````
