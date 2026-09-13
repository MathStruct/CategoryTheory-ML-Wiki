#definition #example

Given [[Category|categories]] $\mathcal{C}$ and $\mathcal{D}$, their **product** $\mathcal{C} \times \mathcal{D}$ has objects pairs $(c, d)$ and morphisms $(c, d) \to (c', d')$ pairs $(f, g)$ with $f : c \to c'$ and $g : d \to d'$; composition and identities are componentwise: $(f, g) \mathbin{;} (f', g') = (f \mathbin{;} f', g \mathbin{;} g')$, $\mathrm{id}_{(c,d)} = (\mathrm{id}_c, \mathrm{id}_d)$ ([[7S Chapter 3 Exercises#Exercise 3.90|7S Exercise 3.90]]).

> Sources: 7 Sketches Example 3.89, Exercise 3.90, Example 1.56; DaoFP §8.1 ("Product categories"), §10.1–10.2; [[Product of Enriched Categories]].

- $\underline{\mathbf{1}} \times \mathcal{C} \cong \mathcal{C}$; for preorders $P, Q$ the product category is the [[Product Preorder]] ([[7S Chapter 3 Exercises#Exercise 3.90|7S Exercise 3.90]]).
- $\mathcal{C} \times \mathcal{D}$ is the [[Product]] in [[Category of Categories|$\mathbf{Cat}$]], and $\mathcal{C} \times \mathcal{C} \simeq [\mathbf{2}, \mathcal{C}]$, the [[Functor Category]] from the discrete two-object category; hence the [[Diagonal Functor]] $\Delta : \mathcal{C} \to \mathcal{C} \times \mathcal{C}$ is the [[Constant Functor|constant-diagram]] functor and $(+) \dashv \Delta \dashv (\times)$ (DaoFP §10.2).
- Functors out of $\mathcal{C} \times \mathcal{D}$ are [[Bifunctor|bifunctors]]; functors $\mathcal{C}^{\mathrm{op}} \times \mathcal{D} \to \mathbf{Set}$ are [[Profunctor|profunctors]], e.g. the [[Hom Functor]]. $\mathbf{Cat}$ is [[Cartesian Closed Category|cartesian closed]]: $\mathbf{Cat}(\mathcal{C} \times \mathcal{D}, \mathcal{E}) \cong \mathbf{Cat}(\mathcal{C}, [\mathcal{D}, \mathcal{E}])$ (DaoFP §10.1), which is how the [[Yoneda Embedding]] is obtained by currying the hom-functor.

````tabs
tab: Julia
```julia
struct ProductCat{C<:Category, D<:Category} <: Category{Tuple, Tuple}
  c::C; d::D
end
Categories.dom(p::ProductCat, (f, g)) = (dom(p.c, f), dom(p.d, g))
Categories.codom(p::ProductCat, (f, g)) = (codom(p.c, f), codom(p.d, g))
Categories.compose(p::ProductCat, (f, g), (f′, g′)) = (compose(p.c, f, f′), compose(p.d, g, g′))
Categories.id(p::ProductCat, (x, y)) = (id(p.c, x), id(p.d, y))
```
tab: Lean
```lean
#check CategoryTheory.prod       -- instance : Category (C × D), morphisms are pairs
#check CategoryTheory.Functor.prod
example {C D : Type} [CategoryTheory.Category C] [CategoryTheory.Category D] :
    CategoryTheory.Category (C × D) := inferInstance
```
tab: Haskell
```haskell
-- the product of two categories: pairs of arrows
newtype (cat1 :*: cat2) (a, b) (c, d) = ProdArr (cat1 a c, cat2 b d)
-- with componentwise identity and composition
```
````
