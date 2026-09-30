#definition #theorem #example

The **category of elements** $\int F$ (also $\mathrm{El}(F)$) of a set-valued functor $F : \mathcal{C} \to \mathbf{Set}$ has as objects pairs $(c, x)$ with $x \in F c$ and as morphisms $(c, x) \to (c', x')$ the morphisms $f : c \to c'$ with $F f (x) = x'$. The projection $\pi_F : \int F \to \mathcal{C}$ is a discrete opfibration. Dually for a presheaf $G : \mathcal{C}^{\mathrm{op}} \to \mathbf{Set}$ (with $G f(x') = x$).

> Sources: 7 Sketches Remark 3.100 (the pullback of instances along a functor is a pullback in $\mathbf{Cat}$ via categories of elements), §3.3.3 (an instance as a "bunch of tables" — its rows are the elements); DaoFP §9.8 (every presheaf is a colimit of representables, indexed by its category of elements), §11.2 (fibrations); Kittenlab Lecture 6 ([[C-Set]] instances); CTfS §4.6.2 (Definition 4.6.2.1, Example 4.6.2.2, Application 4.6.2.3, Exercises 4.6.2.4–4.6.2.5), Example 4.6.4.2

- A [[C-Set]] $F$ is recovered from $\int F \to \mathcal{C}$: the elements over $c$ are the rows of table $c$, and $F f$ is what the foreign key $f$ does to rows. In Catlab an ACSet *is* stored as its category of elements (parts and subpart functions).
- **Density**: $F \cong \mathrm{colim}_{(c, x) \in \int F} \mathcal{C}(c, -)$ — every co-presheaf is a colimit of [[Representable Functor|representables]] indexed by its elements ([[Yoneda Lemma]], [[Ninja Yoneda Lemma|co-Yoneda]]).
- $\int F$ is the [[Comma Category]] $* \downarrow F$ (with $* : \mathbf{1} \to \mathbf{Set}$ the point), and a [[Slice Category|slice]] of the presheaf category: $\widehat{\mathcal{C}}/F \simeq \widehat{\int F}$.
- **RDF triple stores** (CTfS Application 4.6.2.3). The web's Resource Description Framework stores data schema-free as triples $\langle \text{subject}, \text{predicate}, \text{object} \rangle$, e.g. `⟨A01 occurredOn D13114⟩`, `⟨D13114 hasYear 2013⟩`, `⟨P44 FirstName Barack⟩`. Converting a database instance $I : \mathcal{C} \to \mathbf{Set}$ into a triple store *is* the category of elements: every arrow $(c, x) \xrightarrow{f} (c', I f(x))$ of $\int I$ gives the triple $\langle x, f, I f(x) \rangle$. For the employee database of [[Database Schema]] this yields `⟨101 manager 103⟩`, `⟨102 first Bertrand⟩`, `⟨q10 secretary 101⟩`, …
- **Histograms and indexed sets** (CTfS Example 4.6.2.2). For a set $A$ viewed as a discrete category, a functor $S : A \to \mathbf{Set}$ is an [[Indexed Set|$A$-indexed set]] and $\int S = \coprod_{a \in A} S_a$ with $\pi_S$ sending each element to its index — e.g. people binned by city: $S_{\mathrm{BOS}} = \{\text{Abby}, \text{Bob}, \text{Casandra}\}$, $S_{\mathrm{NYC}} = \varnothing$, $S_{\mathrm{LA}} = \{\text{John}, \text{Jim}\}$, $S_{\mathrm{DC}} = \{\text{Abby}, \text{Carla}\}$. The construction converts indexed sets into sets over $A$ (a [[Slice Category|slice]]).
- **State machines** ([[CTfS Chapter 4 Exercises#Exercise 4.6.2.5|CTfS Exercise 4.6.2.5]]): for a [[Finite State Machine]], i.e. a functor $\mathrm{List}(\Sigma) \to \mathbf{Set}$, the category of elements has the states as objects and a morphism for every input word — it is the free category on the familiar state-transition picture.
- Grothendieck construction: for $F : \mathcal{C} \to \mathbf{Cat}$ the same recipe produces a (non-discrete) fibration ([[Dependent Type]]).

````tabs
tab: Julia
**Docs:** [ACSets API](https://algebraicjulia.github.io/ACSets.jl/stable/api/) · [Graphs](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/graphs/) · [Vignette: category of elements](https://algebraicjulia.github.io/Catlab.jl/v0.16/generated/sketches/cat_elements/) — Kittenlab [Lecture 6](https://algebraicjulia.github.io/Kittenlab.jl/lecture6.html)
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
