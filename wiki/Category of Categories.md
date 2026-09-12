#definition #theorem #example

**$\mathbf{Cat}$** is the [[Category]] whose objects are (small) categories and whose morphisms are [[Functor|functors]]; identities are the identity functors and composition is composition of functors ([[7S Exercise 3.43]]). Kittenlab's `KittenC` is "the category of categories and functors implemented in Julia".

> Sources: 7 Sketches Exercise 3.43, 3.82, §3.2.4 ("action in context, structure"); Kittenlab Lecture 4; DaoFP §8.5 ("Category of categories"), §9.9 ("2-category $\mathbf{Cat}$"), §10.1, §10.11.

*Proof it is a category.* $\mathrm{id}_{\mathcal{C}}$ (identity on objects and morphisms) preserves identities and composition; the composite $F \mathbin{;} G$ of functors preserves them since each does; unitality and associativity hold pointwise because they do for functions. $\blacksquare$

- "Size issues": to avoid paradoxes $\mathbf{Cat}$ is the category of *small* categories; "as long as we are not engaged in proofs of existence, we can ignore size problems" (DaoFP).
- The [[Terminal Object]] of $\mathbf{Cat}$ is $\underline{\mathbf{1}}$ ([[7S Exercise 3.82]]); the [[Initial Object]] is $\underline{\mathbf{0}}$; [[Product|products]] are [[Product Category|product categories]]; $\mathbf{Cat}$ is [[Cartesian Closed Category|cartesian closed]] with internal hom the [[Functor Category]] $[\mathcal{C}, \mathcal{D}]$ (DaoFP §10.1: $\mathbf{Cat}(\mathcal{C} \times \mathcal{D}, \mathcal{E}) \cong \mathbf{Cat}(\mathcal{C}, [\mathcal{D}, \mathcal{E}])$).
- $\mathbf{Cat}$ is a **strict [[2-Category]]**: hom-sets are themselves categories (functor categories), with [[Natural Transformation|natural transformations]] as 2-cells; it is enriched over itself (DaoFP §20.1). "Category theory is its own metatheory: the collection of all categories forms a category, but the collection of all rings does not form a ring" (Kittenlab Lecture 7).
- [[Category of Preorders|$\mathbf{Preord}$]] and $\mathbf{Mon}$ are full subcategories; $\mathbf{Set} \rightleftarrows \mathbf{Cat}$ via [[Discrete Category|discrete]]/objects/[[Codiscrete Category|codiscrete]]; $\mathbf{Grph} \rightleftarrows \mathbf{Cat}$ via [[Free Category|Free]]/underlying graph; [[Adjunction|adjunctions]] compose to form $\mathbf{Adj}(\mathbf{Cat})$ (DaoFP §10.10).
- "Levels of abstraction" (DaoFP §10.11): a set is a discrete category; the set is an object of $\mathbf{Set}$; $\mathbf{Set}$ is an object of $\mathbf{Cat}$; functors are objects of $[\mathcal{C}, \mathcal{D}]$; and hom-sets in every category are sets, "completing the circle".

````tabs
tab: Julia
```julia
# Kittenlab src/Functors.jl: KittenC, the category of Julia-implemented categories
struct KittenC <: Category{Category, Functor} end

struct ComposedFunctor{C<:Category, D<:Category, E<:Category} <: Functor{C, E}
  F::Functor{C,D}; G::Functor{D,E}
end
ob_map(FG::ComposedFunctor, x) = ob_map(FG.G, ob_map(FG.F, x))
hom_map(FG::ComposedFunctor, f) = hom_map(FG.G, hom_map(FG.F, f))
Categories.compose(::KittenC, F::Functor, G::Functor) = ComposedFunctor(F, G)

struct IdFunctor{C<:Category} <: Functor{C, C}
  c::C
end
ob_map(::IdFunctor, x) = x
hom_map(::IdFunctor, f) = f
Categories.id(::KittenC, c::Category) = IdFunctor(c)
```
tab: Lean
```lean
#check CategoryTheory.Cat          -- the category of small categories (a bundled Category.{v,u})
#check CategoryTheory.Cat.terminal? -- see `CategoryTheory.Cat.isTerminalPUnit`
#check CategoryTheory.Cat.equivOfIso
-- Cat is a strict 2-category / bicategory:
#check CategoryTheory.Cat.bicategory
```
tab: Haskell
```haskell
-- Haskell has no first-class Cat, but functors compose (Compose) and there is an identity functor;
-- "Cat" is the meta-level: type constructors of kind Type -> Type with Functor instances.
import Data.Functor.Compose (Compose(..))
import Data.Functor.Identity (Identity(..))
```
````
