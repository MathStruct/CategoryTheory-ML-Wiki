#definition #example

Let $\mathcal{C}$ be a [[Category]]. Its **opposite** $\mathcal{C}^{\mathrm{op}}$ has the same objects, $\mathrm{Ob}(\mathcal{C}^{\mathrm{op}}) := \mathrm{Ob}(\mathcal{C})$, and hom-sets $\mathcal{C}^{\mathrm{op}}(c, d) := \mathcal{C}(d, c)$; identities are as in $\mathcal{C}$ and composition is reversed: $g^{\mathrm{op}} \circ f^{\mathrm{op}} = (f \circ g)^{\mathrm{op}}$. "Take any category you already have and reverse all its morphisms; the result is again a category."

> Sources: 7 Sketches Example 3.27, Exercise 3.101, Definition 3.102; DaoFP §8.1 ("Opposite categories"), §5.2 ("Duality"); Kittenlab Lecture 13 ("Duals").

- A [[Functor]] $F : \mathcal{C} \to \mathcal{D}$ has an opposite $F^{\mathrm{op}} : \mathcal{C}^{\mathrm{op}} \to \mathcal{D}^{\mathrm{op}}$, the same on objects and $F^{\mathrm{op}}(f^{\mathrm{op}}) := F(f)^{\mathrm{op}}$ ([[7S Chapter 3 Exercises#Exercise 3.101|7S Exercise 3.101]]).
- **Duality**: every categorical statement has a dual obtained by reversing arrows — [[Terminal Object|terminal]]/[[Initial Object|initial]], [[Product]]/[[Coproduct]], [[Limit]]/[[Colimit]] ("a cocone in $\mathcal{C}$ is a cone in $\mathcal{C}^{\mathrm{op}}$", Definition 3.102), [[Monomorphism|mono]]/[[Epimorphism|epi]], [[Monad]]/[[Comonad]], [[Algebra of an Endofunctor|algebra]]/[[Coalgebra of an Endofunctor|coalgebra]]. Kittenlab: "I could just swap the definition of domain and codomain and formally everything would look the same" — as long as you are clear about the convention.
- Contravariant functors $\mathcal{C}^{\mathrm{op}} \to \mathcal{D}$ ([[Contravariant Functor]]), [[Presheaf|presheaves]] $\mathcal{C}^{\mathrm{op}} \to \mathbf{Set}$, and [[Profunctor|profunctors]] $\mathcal{C}^{\mathrm{op}} \times \mathcal{D} \to \mathbf{Set}$ (DaoFP: $\mathcal{C}^{\mathrm{op}} \times \mathcal{C}$ is one of the two most interesting [[Product Category|product categories]]).
- For preorders: the [[Opposite Preorder]]; for enriched categories: the [[Opposite Enriched Category]] (needs symmetry of $\mathcal{V}$).

````tabs
tab: Julia
```julia
# Kittenlab-style: the opposite of any Category value
struct OppositeCat{Ob,Hom,C<:Category{Ob,Hom}} <: Category{Ob,Hom}
  c::C
end
Categories.dom(o::OppositeCat, f) = codom(o.c, f)
Categories.codom(o::OppositeCat, f) = dom(o.c, f)
Categories.compose(o::OppositeCat, f, g) = compose(o.c, g, f)    # reversed
Categories.id(o::OppositeCat, x) = id(o.c, x)

# Catlab: `op(C)` for a FinCat, and `op` on presentations/GAT expressions
using Catlab
```
tab: Lean
```lean
#check CategoryTheory.Opposite      -- Cᵒᵖ, with objects `op X` and morphisms `f.op`
#check @CategoryTheory.Functor.op   -- F.op : Cᵒᵖ ⥤ Dᵒᵖ
example (C : Type) [CategoryTheory.Category C] (X Y : C) (f : X ⟶ Y) :
    (CategoryTheory.Opposite.op Y ⟶ CategoryTheory.Opposite.op X) := f.op
```
tab: Haskell
```haskell
-- Data.Functor.Contravariant / Control.Category: the opposite of a category
newtype Op cat a b = Op (cat b a)

instance Category cat => Category (Op cat) where
  id = Op id
  Op g . Op f = Op (f . g)      -- reversed composition
```
````
