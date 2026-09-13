#definition #example

The **terminal category** $\mathbf{1}$ has one object $*$ and only the identity morphism. It is the [[Terminal Object]] of the [[Category of Categories|category $\mathbf{Cat}$]]: for every $\mathcal{C}$ there is a unique functor $! : \mathcal{C} \to \mathbf{1}$. A functor $\mathbf{1} \to \mathcal{C}$ is the same as an object of $\mathcal{C}$ ([[Global Element]]); a functor $\mathbf{1} \to \mathbf{Set}$ is a set; $\mathcal{C}^{\mathbf{1}} \simeq \mathcal{C}$.

> Sources: DaoFP §3.1 ("stick-figure categories"), §19.3 (limits as Kan extensions along $! : \mathcal{J} \to \mathbf{1}$), §20.7; 7 Sketches Example 3.30 (the database schema with one table and no columns: instances are sets), §7.4.1 (the one-point space: $\mathbf{Set} = \mathbf{Shv}(1)$); Kittenlab Lecture 9.

- The [[Initial Object]] of $\mathbf{Cat}$ is the empty category $\mathbf{0}$; the [[Walking Arrow]] $\mathbf{2}$ is the next stick figure.
- [[Limit]]s and [[Colimit]]s are [[Kan Extension|Kan extensions]] along $! : \mathcal{J} \to \mathbf{1}$; the [[Constant Functor]] $\Delta_x$ is $X \circ !$ for $X : \mathbf{1} \to \mathcal{C}$ picking $x$.
- As a [[Monoidal Category]] $\mathbf{1}$ is the unit of the product of categories, and a one-object monoidal category is a [[Monoid]].

````tabs
tab: Julia
```julia
using Catlab
@present One(FreeCategory) begin X::Ob end          # the terminal category: one object, no generating arrows
# a functor 1 → Graph picks an object: an instance on the one-table schema is a set
@present SchSet(FreeSchema) begin X::Ob end
@acset_type SetInst(SchSet)
S = @acset SetInst begin X = 4 end                   # "a set with 4 elements"
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
#check @CategoryTheory.Discrete PUnit             -- the terminal category as Discrete PUnit
#check @CategoryTheory.Functor.star               -- the unique functor C ⥤ Discrete PUnit
```
tab: Haskell
```haskell
-- the terminal category has one object; a functor out of it is just an object
data One = Star
pick :: c -> (One -> c)
pick x Star = x
```
````
