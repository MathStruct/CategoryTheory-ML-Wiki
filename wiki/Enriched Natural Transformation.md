#definition

Let $F, G : \mathcal{C} \to \mathcal{D}$ be [[Enriched Functor|$\mathcal{V}$-functors]] between $\mathcal{V}$-[[Enriched Category|categories]]. A **$\mathcal{V}$-natural transformation** $\nu : F \Rightarrow G$ has components that are "global elements" of hom-objects, $\nu_a : I \to \mathcal{D}(Fa, Ga)$, satisfying the naturality condition expressed as a commuting hexagon in $\mathcal{V}$:

$$
\circ \circ (\nu_b \otimes F_{ab}) \circ \lambda^{-1} = \circ \circ (G_{ab} \otimes \nu_a) \circ \rho^{-1} : \mathcal{C}(a, b) \to \mathcal{D}(Fa, Gb),
$$

equivalently $\mathcal{D}(Fa, \nu_b) \circ F_{ab} = \mathcal{D}(\nu_a, Gb) \circ G_{ab}$ using the enriched [[Hom Functor|hom-functor]]'s action on global elements. For $\mathcal{V} = \mathbf{Set}$ this is the ordinary naturality square (pick $f \in \mathcal{C}(a, b)$, then $\nu_b \circ Ff = Gf \circ \nu_a$). $\mathcal{V}$-natural transformations form a *set* $\mathcal{V}\text{-}\mathrm{nat}(F, G)$; the enriched [[End]] $\int_a \mathcal{D}(Fa, Ga)$ gives instead the *object* of natural transformations in $\mathcal{V}$.

> Sources: DaoFP §20.3 ("$\mathcal{V}$-Natural Transformations"), §17.3 (natural transformations as an end), §20.4 (enriched Yoneda); 7 Sketches Remark 2.71, Example 3.57 (for preorders: at most one, existing iff $F \leq G$ pointwise).

For $\mathcal{V} = \mathbf{Bool}$ a $\mathbf{Bool}$-natural transformation between monotone maps $F, G : P \to Q$ exists iff $F(p) \leq G(p)$ for all $p$ (there is at most one). Enriched natural transformations are the 2-cells of the 2-category $\mathcal{V}\text{-}\mathbf{Cat}$, and [[Weighted Limit|weighted limits]], enriched [[Kan Extension|Kan extensions]] and the enriched [[Yoneda Lemma]] $[\mathcal{C}, \mathcal{V}](\mathcal{C}(a, -), F) \cong F a$ are phrased in terms of them.

````tabs
tab: Lean
```lean
-- Mathlib: transformations between enriched functors (via the underlying / forgetful structure)
#check CategoryTheory.EnrichedFunctor
#check CategoryTheory.EnrichedNatTrans    -- (if available in your Mathlib version: `CategoryTheory.Enriched.NatTrans`)
```
tab: Haskell
```haskell
-- in Hask (self-enriched), an enriched natural transformation is just a polymorphic function,
-- but its components are *elements of the internal hom* f a -> g a:
type EnrichedNat f g = forall a. f a -> g a
```
````
