#definition #theorem #example

The **category of elements** $\int F$ (also $\mathrm{El}(F)$) of a set-valued functor $F : \mathcal{C} \to \mathbf{Set}$ has as objects pairs $(c, x)$ with $x \in F c$ and as morphisms $(c, x) \to (c', x')$ the morphisms $f : c \to c'$ with $F f (x) = x'$. The projection $\pi_F : \int F \to \mathcal{C}$ is a discrete opfibration. Dually for a presheaf $G : \mathcal{C}^{\mathrm{op}} \to \mathbf{Set}$ (with $G f(x') = x$).

> Sources: 7 Sketches Remark 3.100 (the pullback of instances along a functor is a pullback in $\mathbf{Cat}$ via categories of elements), §3.3.3 (an instance as a "bunch of tables" — its rows are the elements); DaoFP §9.8 (every presheaf is a colimit of representables, indexed by its category of elements), §11.2 (fibrations); Kittenlab Lecture 6 ([[C-Set]] instances).

- A [[C-Set]] $F$ is recovered from $\int F \to \mathcal{C}$: the elements over $c$ are the rows of table $c$, and $F f$ is what the foreign key $f$ does to rows. In Catlab an ACSet *is* stored as its category of elements (parts and subpart functions).
- **Density**: $F \cong \mathrm{colim}_{(c, x) \in \int F} \mathcal{C}(c, -)$ — every co-presheaf is a colimit of [[Representable Functor|representables]] indexed by its elements ([[Yoneda Lemma]], [[Ninja Yoneda Lemma|co-Yoneda]]).
- $\int F$ is the [[Comma Category]] $* \downarrow F$ (with $* : \mathbf{1} \to \mathbf{Set}$ the point), and a [[Slice Category|slice]] of the presheaf category: $\widehat{\mathcal{C}}/F \simeq \widehat{\int F}$.
- Grothendieck construction: for $F : \mathcal{C} \to \mathbf{Cat}$ the same recipe produces a (non-discrete) fibration ([[Dependent Type]]).

````tabs
tab: Julia
```julia
using Catlab
G = path_graph(Graph, 3)
# the category of elements of the graph G (as a C-set): its objects are the parts
elems = [(ob, i) for ob in (:V, :E) for i in parts(G, ob)]          # 5 objects
# morphisms: for each edge e, src: (E,e) → (V, src(e)) and tgt: (E,e) → (V, tgt(e))
arrows = [((:E, e), :src, (:V, G[e, :src])) for e in parts(G, :E)] ∪
         [((:E, e), :tgt, (:V, G[e, :tgt])) for e in parts(G, :E)]
length(elems), length(arrows)                                        # (5, 4)
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
#check @CategoryTheory.Functor.Elements        -- F.Elements for F : C ⥤ Type
#check @CategoryTheory.CategoryOfElements.π    -- the projection to C
#check @CategoryTheory.Grothendieck            -- Grothendieck construction for F : C ⥤ Cat
```
tab: Haskell
```haskell
-- the category of elements of a finite Set-valued functor, by enumeration
data Elem c x = Elem c x                        -- an object (c, x) with x ∈ F c
-- a morphism (c, x) -> (c', x') is an f : c -> c' with fmapF f x == x'
```
````
