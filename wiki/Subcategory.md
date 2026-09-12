#definition #example

If $\mathcal{C}$ is a [[Category]], a **subcategory** $\mathcal{D}$ consists of a subset $\mathcal{D}_0 \subseteq \mathcal{C}_0$ of objects and, for each $x, y \in \mathcal{D}_0$, a subset $\mathrm{Hom}_{\mathcal{D}}(x,y) \subseteq \mathrm{Hom}_{\mathcal{C}}(x,y)$ such that all identities are in $\mathcal{D}$ and composites of morphisms in $\mathcal{D}$ are in $\mathcal{D}$. $\mathcal{D}$ is **wide** if $\mathcal{D}_0 = \mathcal{C}_0$ and **full** if $\mathrm{Hom}_{\mathcal{D}}(x,y) = \mathrm{Hom}_{\mathcal{C}}(x,y)$ for all $x, y \in \mathcal{D}_0$; the only wide full subcategory is $\mathcal{C}$ itself.

> Sources: Kittenlab Lecture 5; DaoFP §9.5 (full subcategory on a weakly terminal set); 7 Sketches Example 3.74 (abelian groups in groups).

**Subtlety (Kittenlab).** $\mathsf{Preorder}$ is a full subcategory of $\mathbf{Cat}$ — but a preorder-as-(set, relation) is not *literally* a thin category; there is only an injective [[Functor]] into $\mathbf{Cat}$. So the categorical notion of subobject is generalized: a subcategory of $\mathcal{C}$ is any category with an injective (faithful, injective-on-objects) functor into $\mathcal{C}$, and "we should not distinguish between isomorphic objects". Everyone then follows convention and ignores the pedantry. Compare [[Subobject]] and [[Monomorphism]].

**Examples.** [[Category of Finite Sets|$\mathbf{FinSet}$]] $\subseteq \mathbf{Set}$ (full); [[Category of Preorders|$\mathbf{Preord}$]] $\subseteq \mathbf{Cat}$ (full); $\mathbf{Ab} \subseteq \mathbf{Grp}$ (full, with a left adjoint — a *reflective* subcategory); injections form a wide non-full subcategory of $\mathbf{Set}$; the [[Yoneda Embedding]] exhibits $\mathcal{C}$ as a full subcategory of presheaves ([[Representable Functor|representables]]).

````tabs
tab: Lean
```lean
#check CategoryTheory.FullSubcategory      -- FullSubcategory (Z : C → Prop)
#check CategoryTheory.InducedCategory      -- induced category along a function
#check CategoryTheory.Functor.Faithful
#check CategoryTheory.Functor.Full
```
````
