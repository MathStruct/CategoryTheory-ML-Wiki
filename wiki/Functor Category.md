#definition #example #theorem

For [[Category|categories]] $\mathcal{C}, \mathcal{D}$, the **functor category** $\mathcal{D}^{\mathcal{C}}$ (also $[\mathcal{C}, \mathcal{D}]$ or $\mathbf{Fun}(\mathcal{C}, \mathcal{D})$) has [[Functor|functors]] $F : \mathcal{C} \to \mathcal{D}$ as objects and [[Natural Transformation|natural transformations]] as morphisms; composition is vertical composition of natural transformations (componentwise) and identities are $(\mathrm{id}_F)_c = \mathrm{id}_{F(c)}$ ([[7S Chapter 3 Exercises#Exercise 3.55|7S Exercise 3.55]]).

> Sources: 7 Sketches Definition 3.54, Examples 3.56, 3.57, Definition 3.60; Kittenlab Lecture 7 (`FunctorCat`), 8, 9, 12; DaoFP §9.3 ("Functor categories"), §9.7, §10.1, §10.4.

- "What is an arrow in one category could be an object in another": in $\mathbf{Cat}$ functors are arrows; in $[\mathcal{C}, \mathcal{D}]$ they are dots (DaoFP).
- $\mathcal{C}\text{-}\mathbf{Inst} := \mathbf{Set}^{\mathcal{C}}$ is the category of [[C-Set|database instances]] (Definition 3.60); $\mathbf{Set}^{\underline{1}} \simeq \mathbf{Set}$; $\mathbf{Set}^{\mathsf{Gr}} = \mathbf{Grph}$ ([[Category of Graphs]]); $[\mathcal{C}^{\mathrm{op}}, \mathbf{Set}]$ is the category of [[Presheaf|presheaves]] and $[\mathcal{C}, \mathbf{Set}]$ of co-presheaves.
- $\mathbb{N}^{\mathbb{N}}$ for the preorder $\mathbb{N}$ is the preorder of monotone maps (Example 3.57); in general $[\mathcal{C}, P]$ is a preorder when $P$ is.
- $[\mathcal{J}, \mathcal{C}]$ is the category of [[Diagram|diagrams]] of shape $\mathcal{J}$; [[Limit|limits]] and [[Colimit|colimits]] are adjoints to $\Delta : \mathcal{C} \to [\mathcal{J}, \mathcal{C}]$; $[\mathbf{2}, \mathcal{C}] \cong \mathcal{C} \times \mathcal{C}$.
- $[\mathcal{C}, \mathcal{D}]$ is the [[Exponential Object|internal hom]] of the [[Cartesian Closed Category|cartesian closed]] category [[Category of Categories|$\mathbf{Cat}$]], so functors can be curried (DaoFP §9.7, §10.1: $\mathbf{Cat}(\mathcal{C} \times \mathcal{D}, \mathcal{E}) \cong \mathbf{Cat}(\mathcal{C}, [\mathcal{D}, \mathcal{E}])$); this is how the [[Yoneda Embedding]] arises from the [[Hom Functor]].
- Limits and colimits in $[\mathcal{C}, \mathcal{D}]$ are computed pointwise when $\mathcal{D}$ has them (products/coproducts/pushouts of graphs, Kittenlab). $[\mathcal{C}, \mathbf{Set}]$ is a [[Topos]].
- The set of natural transformations is an [[End]]: $[\mathcal{C}, \mathcal{D}](F, G) \cong \int_a \mathcal{D}(Fa, Ga)$ (DaoFP §17.3). The [[Yoneda Lemma]]: $[\mathcal{C}, \mathbf{Set}](\mathcal{C}(a, -), F) \cong F(a)$.

````tabs
tab: Julia
```julia
# Kittenlab src/NaturalTransformations.jl
struct FunctorCat{C<:Category, D<:Category} <: Category{Functor{C,D}, NaturalTransformation{C,D}}
  c::C; d::D
end
# compose = vertical composition of components, id = identity transformation (see Natural Transformation)

# Catlab: the category of C-sets for a schema is available through ACSet types;
# hom-sets are computed by `homomorphisms(X, Y)`
```
tab: Lean
```lean
open CategoryTheory in
example {C D : Type} [Category C] [Category D] : Category (C ⥤ D) := inferInstance   -- Functor.category
#check CategoryTheory.Functor.category
#check CategoryTheory.currying          -- (C × D ⥤ E) ≌ (C ⥤ D ⥤ E)
```
tab: Haskell
```haskell
-- objects: Functor instances f; morphisms: forall a. f a -> g a; composition is (.)
newtype (:~>) f g = Nat (forall a. f a -> g a)
idNat :: f :~> f
idNat = Nat id
compNat :: (g :~> h) -> (f :~> g) -> (f :~> h)
compNat (Nat b) (Nat a) = Nat (b . a)
```
````
