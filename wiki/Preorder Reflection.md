#definition #example

Given any [[Category]] $\mathcal{C}$, its **preorder reflection** is the [[Preorder]] $(\mathrm{Ob}(\mathcal{C}), \leq)$ with $c_1 \leq c_2$ iff $\mathcal{C}(c_1, c_2) \neq \varnothing$ — it "destroys the distinction between any two parallel morphisms": one, two, fifty or infinitely many morphisms all look the same, but *some* versus *none* is still seen.

> Sources: 7 Sketches §3.2.3, Exercise 3.22, Remark 3.23; Kittenlab Lecture 5 (functor 2 in the list of functors between $\mathbf{Cat}$ and $\mathsf{Preorder}$).

- The preorder reflection of the one-object category $\mathbb{N}$ (Example 3.13) is $\underline{1}$ ([[7S Chapter 3 Exercises#Exercise 3.22|7S Exercise 3.22]]).
- "Considering a preorder as a category is right adjoint to turning a category into a preorder by preorder reflection" — an [[Adjunction]] $\mathrm{Refl} \dashv \mathrm{Incl} : \mathbf{Preord} \rightleftarrows \mathbf{Cat}$; a statement "you might not understand exactly, but it's true".
- A [[Diagram]] $D : \mathcal{J} \to \mathcal{C}$ commutes iff it factors through the preorder reflection of $\mathcal{J}$ (footnote to Definition 3.51).
- The [[Free Category]] on a graph and the preorder presented by a [[Hasse Diagram]] are the two ends of the spectrum of [[Presentation of a Category|presentations]] with the same graph.

````tabs
tab: Julia
```julia
# preorder reflection of a finite category given by an object list and a hom-nonempty predicate
function preorder_reflection(objects, homs_nonempty)
  Dict((x, y) => homs_nonempty(x, y) for x in objects, y in objects)   # the ≤ relation as a table
end
# Catlab: is_hom_equal-free version for a FinCat with finite hom-sets
```
tab: Lean
```lean
-- the thin category on Ob C with x ≤ y iff Nonempty (x ⟶ y)
def preorderReflection (C : Type u) [CategoryTheory.Category C] : Preorder C where
  le x y := Nonempty (x ⟶ y)
  le_refl x := ⟨CategoryTheory.CategoryStruct.id x⟩
  le_trans _ _ _ := fun ⟨f⟩ ⟨g⟩ => ⟨f ≫ g⟩
```
````
