#definition #example

A **diagram** $D$ in a [[Category]] $\mathcal{C}$ is a [[Functor]] $D : \mathcal{J} \to \mathcal{C}$ from a category $\mathcal{J}$, the **indexing category** (or *shape*). It is drawn as a [[Graph]] whose vertices and arrows are labeled by objects and morphisms of $\mathcal{C}$. The diagram **commutes** if $D(f) = D(f')$ for every parallel pair $f, f' : a \to b$ in $\mathcal{J}$ — equivalently, $D$ factors through the [[Preorder Reflection]] of $\mathcal{J}$.

> Sources: 7 Sketches Definition 3.51, Eq. (3.50), Definition 3.92; Kittenlab Lecture 6 ("a commutative diagram is a way of writing an equation between composites of morphisms"), 9; DaoFP §3 (commuting diagrams as equalities of arrows), §9.4–9.5 ("$D$ stands for diagram").

- The naturality square of a [[Natural Transformation]] is a commutative diagram of shape "square" (Eq. 3.50).
- A diagram of shape the discrete $\mathbf{2}$ is a pair of objects; of shape the [[Walking Arrow]], a morphism; of shape $\bullet \to \bullet \leftarrow \bullet$, a [[Cospan]]; $\bullet \leftarrow \bullet \to \bullet$, a [[Span]]; $\bullet \rightrightarrows \bullet$, a parallel pair; $\varnothing$, the empty diagram. A $\mathcal{C}$-set is a diagram in $\mathbf{Set}$ of shape a [[Database Schema|schema]].
- [[Cone|Cones]] and [[Cocone|cocones]] are natural transformations $\Delta_x \Rightarrow D$ and $D \Rightarrow \Delta_x$; their universal versions are [[Limit|limits]] and [[Colimit|colimits]]. Diagrams of shape $\mathcal{J}$ in $\mathcal{C}$ form the [[Functor Category]] $[\mathcal{J}, \mathcal{C}]$.
- Kittenlab's `Diagram` type stores a functor out of a [[Free Category|finitely presented category]] as a dictionary of objects and morphisms; Catlab's `FinDomFunctor`/`Diagram` likewise. Equality of morphisms is what commuting diagrams assert: "equality of set elements is the essence of all the commuting diagrams in category theory" (DaoFP Preface).

````tabs
tab: Julia
```julia
# Catlab: a diagram of shape the "pushout span" in FinSet
using Catlab
@present SchSpan(FreeSchema) begin
  (A, B, C)::Ob
  f::Hom(A, B); g::Hom(A, C)
end
D = FinDomFunctor(Dict(:A => FinSet(2), :B => FinSet(3), :C => FinSet(3)),
                  Dict(:f => FinFunction([1, 2], 3), :g => FinFunction([2, 3], 3)),
                  FinCat(SchSpan))
is_functorial(D)
# Catlab also has `Diagram` (a diagram with its shape) and `colimit(D)` / `limit(D)`
```
tab: Lean
```lean
-- a diagram is a functor J ⥤ C
#check CategoryTheory.Limits.WalkingParallelPair    -- shape for (co)equalizers
#check CategoryTheory.Limits.WalkingSpan            -- shape for pushouts
#check CategoryTheory.Limits.WalkingCospan          -- shape for pullbacks
```
tab: Haskell
```haskell
-- a diagram as a finite category (graph + equations) mapped into a category, stored per generator
data Diagram ob hom = Diagram { objs :: [(String, ob)], homs :: [(String, hom)] }
```
````
