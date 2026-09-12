#definition #example

Let $\mathcal{X}$ and $\mathcal{Y}$ be $\mathcal{V}$-[[Enriched Category|categories]]. A **$\mathcal{V}$-functor** $F : \mathcal{X} \to \mathcal{Y}$ consists of a function $F : \mathrm{Ob}(\mathcal{X}) \to \mathrm{Ob}(\mathcal{Y})$ such that
$$\mathcal{X}(x_1, x_2) \leq \mathcal{Y}(F(x_1), F(x_2)) \quad\text{for all } x_1, x_2 \in \mathrm{Ob}(\mathcal{X}).$$

> Sources: 7 Sketches Definition 2.69, Examples 2.70, 2.72, Exercise 2.73; DaoFP §20.2; the ordinary case is [[Functor]].

The analogy: *preorder is to $\mathbf{Bool}$-category as [[Monotone Map]] is to $\mathbf{Bool}$-functor* (Example 2.70). Likewise a [[Cost]]-functor between [[Lawvere Metric Space|Lawvere metric spaces]] is a function with $d_X(x_1, x_2) \geq d_Y(F x_1, F x_2)$: a **1-Lipschitz** (distance non-increasing) map (Example 2.72) — "a friend from the theory of metric spaces". A **dagger** $\mathcal{V}$-category is one where the identity is a $\mathcal{V}$-functor $\mathcal{X} \to \mathcal{X}^{\mathrm{op}}$ ([[7S Exercise 2.73]]).

## In a monoidal category (DaoFP §20.2)

When $\mathcal{V}$ is a [[Monoidal Category]], a $\mathcal{V}$-functor maps objects to objects and hom-objects to hom-objects via $\mathcal{V}$-morphisms $F_{ab} : \mathcal{C}(a, b) \to \mathcal{D}(F a, F b)$ that commute with composition and identity (diagrams in $\mathcal{V}$). Both categories must be enriched over the *same* $\mathcal{V}$. Examples:
- The [[Hom Functor]] $\mathrm{Hom} : \mathcal{C}^{\mathrm{op}} \otimes \mathcal{C} \to \mathcal{V}$ of a category enriched in a [[Monoidal Closed Category|closed]] $\mathcal{V}$ (treating $\mathcal{V}$ as self-enriched); its action on hom-objects $\mathcal{C}(b,a) \otimes \mathcal{C}(a',b') \to [\mathcal{C}(a,a'), \mathcal{C}(b,b')]$ is built by currying and composing twice. Lifting a global element $f : I \to \mathcal{C}(a, b)$ gives $\mathcal{C}(c, f) : \mathcal{C}(c, a) \to \mathcal{C}(c, b)$ as $\lambda^{-1}$, then $f \otimes \mathrm{id}$, then $\circ$.
- Enriched [[C-Set|co-presheaves]] $\mathcal{C} \to \mathcal{V}$ with $F_{ab} : \mathcal{C}(a,b) \to [F a, F b]$; enriched [[Profunctor|profunctors]] $\mathcal{C}^{\mathrm{op}} \otimes \mathcal{D} \to \mathcal{V}$.
- $\otimes : \mathcal{V} \otimes \mathcal{V} \to \mathcal{V}$ is a $\mathcal{V}$-functor when $\mathcal{V}$ is closed ([[DaoFP Exercise 20.2.2]]).
- A Haskell `Functor` is an enriched endofunctor of $\mathbf{Hask}$ (self-enriched): `fmap :: (a -> b) -> (f a -> f b)` maps *internal* homs. In a self-enriched category, enriched endofunctors are exactly [[Functorial Strength|strong]] endofunctors: strength gives $[a,b] \otimes F a \xrightarrow{\sigma} F([a,b] \otimes a) \xrightarrow{F \varepsilon} F b$, and conversely enrichment gives strength via the coevaluation $\eta_{ab} : a \to [b, a \otimes b]$ — in Haskell `strength (a, bs) = fmap (a,) bs`.

````tabs
tab: Julia
```julia
# check a V-functor between finite V-categories given as an object map (Dict)
function is_vfunctor(X::VCategory, Y::VCategory, F::Dict)
  ix = Dict(o => i for (i, o) in enumerate(X.objects)); iy = Dict(o => i for (i, o) in enumerate(Y.objects))
  all(leq(X.base, X.hom[ix[a], ix[b]], Y.hom[iy[F[a]], iy[F[b]]]) for a in X.objects, b in X.objects)
end
```
tab: Lean
```lean
#check CategoryTheory.EnrichedFunctor      -- EnrichedFunctor V C D
-- Mathlib: `Monotone.functor` is the Bool-enriched case; `LipschitzWith 1` the Cost case
#check LipschitzWith
```
tab: Haskell
```haskell
-- a V-functor on finite V-categories: an object map satisfying hom x y <= hom (F x) (F y)
isVFunctor :: (MonoidalPreorder v, Eq o, Eq o') => VCat v o -> VCat v o' -> (o -> o') -> Bool
isVFunctor (VCat os h) (VCat _ h') f = and [ leq (h x y) (h' (f x) (f y)) | x <- os, y <- os ]

-- Hask-enriched endofunctor = Functor; enrichment gives strength for free
strength :: Functor f => (a, f b) -> f (a, b)
strength (a, bs) = fmap (\b -> (a, b)) bs
```
````
