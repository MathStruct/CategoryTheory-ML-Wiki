#definition #example

A **presheaf** on a [[Category]] $\mathcal{C}$ is a [[Contravariant Functor|contravariant]] set-valued functor $F : \mathcal{C}^{\mathrm{op}} \to \mathbf{Set}$; a **co-presheaf** is a covariant one, $\mathcal{C} \to \mathbf{Set}$ (a [[C-Set]]). The names come from algebraic topology. Presheaves and natural transformations form the [[Functor Category]] $[\mathcal{C}^{\mathrm{op}}, \mathbf{Set}]$, written $\widehat{\mathcal{C}}$ or $\mathbf{Psh}(\mathcal{C})$.

> Sources: DaoFP §9.7; 7 Sketches §7.3.1 ("Presheaves"), Definition 7.28, §7.4; Kittenlab Lecture 6 ("copresheaf is just unnecessarily fancy"), 12.

- The [[Yoneda Embedding]] $x \mapsto \mathcal{C}(-, x)$ lands in presheaves; [[Representable Functor|representables]] are dense in $\widehat{\mathcal{C}}$ (every presheaf is a [[Colimit]] of representables, DaoFP §9.8, §17).
- For $\mathcal{C} = \mathrm{Op}(X)$ the poset of open sets of a [[Topological Space]], a presheaf assigns to each open $U$ a set $F(U)$ of "sections" and to $V \subseteq U$ a restriction map $F(U) \to F(V)$; a [[Sheaf]] is a presheaf satisfying a gluing condition (7 Sketches §7.3). Presheaf categories and sheaf categories are [[Topos|toposes]].
- On a [[Preorder]] $P$, presheaves valued in $\mathbb{B}$ are *lower sets*, co-presheaves are [[Upper Set|upper sets]].
- [[Data Migration Functor|Data migration]] and [[Kan Extension|Kan extensions]] move (co)presheaves between categories.

````tabs
tab: Lean
```lean
example (C : Type) [CategoryTheory.Category C] : Type _ := Cᵒᵖ ⥤ Type   -- presheaves on C
#check CategoryTheory.yoneda
#check TopCat.Presheaf                 -- presheaves on a topological space
```
tab: Haskell
```haskell
-- a presheaf on Hask (contravariant functor); representable ones are (-> a)
class Contravariant f where contramap :: (b -> a) -> f a -> f b
newtype Op a x = Op (x -> a)
instance Contravariant (Op a) where contramap f (Op g) = Op (g . f)
```
````
