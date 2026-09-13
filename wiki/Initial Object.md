#definition #example

An object $0$ of a [[Category]] is **initial** if for every object $a$ there is a unique arrow $¡_a : 0 \to a$ — the dual of a [[Terminal Object]]. DaoFP: "the initial object is the source of everything"; as a type it is `Void` in Haskell, with the unique function `absurd :: Void -> a`; in logic it is falsehood $\bot$, "you can prove anything starting from false premises" ("if wishes were horses, beggars would ride").

> Sources: DaoFP §1.2–1.3, §8.1, Exercise 9.8.2; Kittenlab Lecture 9 ("If $\mathsf{D}$ is the empty category … this is called an initial object"; in $\mathbf{Set}$ the empty set); 7 Sketches §6.2.1 (Definition 6.2, Examples 6.3–6.5), Exercise 1.25.

- In $\mathbf{Set}$: $\varnothing$ (unique empty function); a map *into* $\varnothing$ forces the domain empty ([[7S Chapter 1 Exercises#Exercise 1.25|7S Exercise 1.25]]) — in a [[Cartesian Closed Category]] the initial object is *strict*: "we'll assume it has no incoming arrows other than its identity, so `Void` has no elements" and is empty.
- In a [[Preorder]]: a bottom element, the empty [[Join]]; in $\mathbf{Cat}$: the empty category; in $\mathbf{Grph}$: the empty graph; in $\mathbf{Mon}$: the trivial monoid.
- The initial object is the [[Colimit]] of the empty [[Diagram]] and the unit of [[Coproduct|coproducts]]; every [[Colimit]] is an initial object in a category of [[Cocone|cocones]]; an [[Initial Algebra]] is an initial object in a category of [[Algebra of an Endofunctor|algebras]] ("$\mathbb{N}$ is the initial algebra of $1 + X$").
- The functor $\mathcal{C} \to \mathbf{Set}$, $c \mapsto \{c\}$ is [[Representable Functor|representable]] iff $\mathcal{C}$ has an initial object; the constant-$1$ functor is represented by $0$ (DaoFP Exercises 9.8.2, 9.8.4).

````tabs
tab: Julia
```julia
using Catlab
I0 = initial(FinSet{Int}); ob(I0)    # FinSet(0)
create(I0, FinSet(3))                # the unique FinFunction 0 → 3
```
tab: Lean
```lean
#check CategoryTheory.Limits.IsInitial
#check CategoryTheory.Limits.initial.to        -- ⊥_ C ⟶ X
example : CategoryTheory.Limits.IsInitial (PEmpty : Type) := CategoryTheory.Limits.Types.isInitialPunit   -- (isInitialOfUnique)
```
tab: Haskell
```haskell
import Data.Void (Void, absurd)
-- absurd :: Void -> a   is the unique arrow out of the initial object
fromVoid :: Void -> Int
fromVoid = absurd
```
````
