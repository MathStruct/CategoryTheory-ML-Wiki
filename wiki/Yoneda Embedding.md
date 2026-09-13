#definition #theorem #proof

The **Yoneda functor** is the [[Hom Functor]] curried in one variable:

$$
\mathcal{Y} : \mathcal{C} \to [\mathcal{C}^{\mathrm{op}}, \mathbf{Set}], \qquad \mathcal{Y}(x) := \mathcal{C}(-, x),
$$

sending $x$ to the [[Presheaf]] of "the totality of views of $x$ from all possible directions", and an arrow $f : x \to y$ to the natural transformation with components $(f \circ -) : \mathcal{C}(z, x) \to \mathcal{C}(z, y)$. Fixing the other variable gives the co-Yoneda functor $\mathcal{C}^{\mathrm{op}} \to [\mathcal{C}, \mathbf{Set}]$, $x \mapsto \mathcal{C}(x, -)$ (Kittenlab's $y_{\mathcal{C}}$, contravariant because of the flip in $\mathrm{Hom}(Y, X) \cong \mathrm{Hom}(y_X, y_Y)$).

> Sources: DaoFP §9.7 ("Yoneda Embedding"), §9.8, §9.10; Kittenlab Lecture 12; 7 Sketches Exercise 1.66 ($\uparrow : P^{\mathrm{op}} \to \mathcal{U}(P)$).

**Theorem.** $\mathcal{Y}$ is **fully faithful**: injective on objects, injective on arrows (faithful), and surjective on hom-sets (full) — an *embedding* of $\mathcal{C}$ into its presheaf category, though not surjective on objects.

*Proof.* Substitute $F = \mathcal{C}(-, y)$ in the [[Yoneda Lemma]]: $[\mathcal{C}^{\mathrm{op}}, \mathbf{Set}](\mathcal{C}(-, x), \mathcal{C}(-, y)) \cong \mathcal{C}(x, y)$, naturally in $x, y$. The map sends $f$ to post-composition $(f \circ -)$ (`toNatural f = (f .)`) and its inverse applies a natural transformation to the identity (`fromNatural alpha = alpha id`). Post-composition preserves identities and composition, $((f \circ g) \circ -) = (f \circ -) \circ (g \circ -)$, hence also isomorphisms: $x \cong y$ iff $\mathcal{C}(-, x) \cong \mathcal{C}(-, y)$. $\blacksquare$

"The presheaf $\mathcal{C}(-, a)$, like a hologram, encodes the totality of views of $a$; the Yoneda embedding tells us that when we combine all these individual holograms we get a perfect hologram of the whole category." The image consists of the [[Representable Functor|representable]] presheaves, which are dense: every presheaf is a [[Colimit]] of representables. $\mathbf{Cat}$ being [[Cartesian Closed Category|cartesian closed]] is what allows currying the hom-functor.

````tabs
tab: Lean
```lean
#check CategoryTheory.yoneda                 -- C ⥤ (Cᵒᵖ ⥤ Type v)
#check CategoryTheory.Yoneda.fullyFaithful   -- yoneda is fully faithful
#check CategoryTheory.coyoneda               -- Cᵒᵖ ⥤ (C ⥤ Type v)
```
tab: Haskell
```haskell
{-# LANGUAGE RankNTypes #-}
-- DaoFP §9.7: the action of the Yoneda embedding on arrows and its inverse
toNatural :: (x -> y) -> (forall z. (z -> x) -> (z -> y))
toNatural f = (f .)

fromNatural :: (forall z. (z -> x) -> (z -> y)) -> (x -> y)
fromNatural alpha = alpha id
```
````
