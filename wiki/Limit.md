#definition #example #theorem

Let $D : \mathcal{J} \to \mathcal{C}$ be a [[Diagram]]. The **limit** of $D$, written $\lim D$ (DaoFP: $\mathrm{Lim}\,D$), is the [[Terminal Object]] in the category $\mathrm{Cone}(D)$ of [[Cone|cones]] over $D$. If $\lim D = (C, c_*)$, $C$ is the **limit object** and $c_j$ the **$j$-th projection**. "Don't worry if the details are unclear; the main point is that terminal objects, maximal elements, meets, products of sets, preorders, and categories are all just terminal objects in different categories."

Equivalently: an object $\mathrm{Lim}\,D$ with a natural isomorphism $[\mathcal{J}, \mathcal{C}](\Delta_x, D) \cong \mathcal{C}(x, \mathrm{Lim}\,D)$ — "for each cone with apex $x$ there is a unique map from $x$ into the limit" (DaoFP §9.5); i.e. $\mathrm{Lim}\,D$ [[Representable Functor|represents]] the presheaf of cones. When all limits of shape $\mathcal{J}$ exist, $\Delta \dashv \mathrm{Lim} : [\mathcal{J}, \mathcal{C}] \to \mathcal{C}$ ([[Adjunction]], DaoFP §10.4).

> Sources: 7 Sketches §3.5 (Definitions 3.79, 3.86, 3.92, Examples 3.93–3.99, Theorem 3.95); DaoFP §9.5 ("Limits and Colimits", "Equalizers", "The existence of the terminal object"), §10.4, §10.7, §17.3 ("Limits as ends"), §19.3 ("Limits as Kan extensions"), §20.5 (weighted limits); Kittenlab Lecture 13 ("limits allow you to make tuple types and to filter").

## Special cases

| shape $\mathcal{J}$ | limit |
|---|---|
| empty | [[Terminal Object]] (Example 3.93) |
| two objects, no arrows | [[Product]] (Example 3.94) |
| $\bullet \to \bullet \leftarrow \bullet$ | [[Pullback]] (Example 3.99) |
| $\bullet \rightrightarrows \bullet$ | [[Equalizer]] (DaoFP) |
| [[Walking Arrow]] $\bullet \to \bullet$ | the source object ([[DaoFP Exercise 9.5.1]]) |
| a [[Preorder]] as $\mathcal{C}$ | [[Meet]] $\bigwedge$ of the diagram's objects |
| $\mathcal{C} = \mathbf{Set}$, finite $\mathcal{J}$ | the tuple formula of [[Finite Limits in Set]] (Theorem 3.95); $\Pi_!(I) = \lim I$ ([[Data Migration Functor]]) |
| $\mathcal{C} = \mathbf{Set}$, any $\mathcal{J}$ | the set of cones with apex $1$: $[\mathcal{J}, \mathbf{Set}](\Delta_1, D) \cong \mathbf{Set}(1, \mathrm{Lim}\,D)$ (DaoFP) |

## Properties

- Limits are unique up to unique isomorphism (Remark 3.85); "the limit".
- In $\mathbf{Set}$ (and any [[Topos]]) all finite limits exist; $\mathbf{Set}$ is **complete** (all small limits: products of arbitrary sets and equalizers of arbitrary sets of arrows). All limits can be built from products and equalizers (DaoFP §9.5). Limits in [[Functor Category|functor categories]] and [[C-Set|C-sets]] are pointwise.
- The [[Hom Functor]] $\mathcal{C}(x, -)$ preserves limits; [[Right Adjoints Preserve Limits]]. Limits are [[End|ends]]: $\mathrm{Lim}\,D \cong \int_j D j$ (DaoFP §17.3), and right [[Kan Extension|Kan extensions]] along $\mathcal{J} \to \underline{1}$ (DaoFP §19.3).
- "Limits allow you to make tuple types and to *filter*": [[Pullback|pullbacks]] select tuples satisfying equations, which is how [[Data Migration Functor|$\Pi$-queries]] work.
- Dual: [[Colimit]] (Definition 3.102: a colimit of $D$ is a limit of $D^{\mathrm{op}} : \mathcal{J}^{\mathrm{op}} \to \mathcal{C}^{\mathrm{op}}$ — "like a compressed file, useful for transmitting quickly but useless unless unpacked", which 7 Sketches does in Chapter 6).

````tabs
tab: Julia
```julia
using Catlab
# limit of a diagram in FinSet: a cospan (pullback), computed by `limit`
f = FinFunction([1, 1, 2, 3], 3); g = FinFunction([1, 3], 3)
L = limit(Cospan(f, g))
apex(L), legs(L)                                 # the limit object and its projections
# the tuple formula: pairs (i, j) with f(i) == g(j)
[(i, j) for i in 1:4, j in 1:2 if f(i) == g(j)]  # [(1,1),(2,1),(4,2)]
ob(terminal(FinSet{Int}))                        # limit of the empty diagram: FinSet(1)
```
tab: Lean
```lean
#check CategoryTheory.Limits.limit         -- limit F for F : J ⥤ C with HasLimit F
#check CategoryTheory.Limits.IsLimit       -- the universal property of a cone
#check CategoryTheory.Limits.limit.π       -- projections
#check CategoryTheory.Limits.limit.lift    -- the unique map from any cone
#check CategoryTheory.Limits.constLimAdj   -- const ⊣ lim
#check CategoryTheory.Limits.Types.limitCone  -- limits in Type are sets of compatible families
```
tab: Haskell
```haskell
-- limits in Hask of a finite diagram: compatible tuples (see Finite Limits in Set)
-- e.g. the pullback of f :: a -> c and g :: b -> c on finite carriers:
pullbackSet :: Eq c => [a] -> [b] -> (a -> c) -> (b -> c) -> [(a, b)]
pullbackSet as bs f g = [ (a, b) | a <- as, b <- bs, f a == g b ]
```
````
