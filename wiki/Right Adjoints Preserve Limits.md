#theorem #proof

**Theorem.** If $L \dashv R$ then $R$ preserves [[Limit|limits]]: $R(\mathrm{Lim}\,D) \cong \mathrm{Lim}(R \circ D)$ for any diagram $D$ whose limit exists ($R$ is **continuous**). Dually $L$ preserves [[Colimit|colimits]]: $L(\mathrm{Colim}\,D) \cong \mathrm{Colim}(L \circ D)$ ($L$ is **cocontinuous**).

> Sources: DaoFP §10.7 ("Properties of Adjunctions"); 7 Sketches Proposition 1.111 ([[Right Adjoints Preserve Meets]]), §7.2.1 (in a topos, $- \times a$ preserves colimits); Kittenlab Lecture 9 (representables and colimits).

*Proof (Yoneda argument, DaoFP).* First, the [[Hom Functor]] preserves limits: a cone over $\mathcal{C}(x, D-)$ in $\mathbf{Set}$ with apex $1$ is a family of arrows $x \to Dj$ commuting with the diagram — a cone over $D$ with apex $x$ — so $\mathrm{Lim}\,\mathcal{C}(x, D-) \cong \mathcal{C}(x, \mathrm{Lim}\,D)$; dually $\mathrm{Lim}\,\mathcal{C}(D-, x) \cong \mathcal{C}(\mathrm{Colim}\,D, x)$. Then, for every $x$,

$$
\mathcal{C}(x, R(\mathrm{Lim}\,D)) \cong \mathcal{D}(Lx, \mathrm{Lim}\,D) \cong \mathrm{Lim}\,\mathcal{D}(Lx, D-) \cong \mathrm{Lim}\,\mathcal{C}(x, RD-) \cong \mathcal{C}(x, \mathrm{Lim}(R \circ D)),
$$

naturally in $x$, so by the [[Yoneda Lemma]] $R(\mathrm{Lim}\,D) \cong \mathrm{Lim}(R \circ D)$. The colimit statement is dual. $\blacksquare$

**Applications.** Distributivity in a [[Cartesian Closed Category]]: $(- \times a)$ is a left adjoint, coproducts are colimits, so $(b + c) \times a \cong b \times a + c \times a$ (DaoFP; the earlier long proof used the currying and sum adjunctions plus Yoneda four times). [[Data Migration Functor|$\Delta_F$]] preserves both limits and colimits (it has both adjoints). Forgetful functors preserve limits (underlying set of a product of groups is the product of sets) but typically not colimits. Left adjoints have no [[Generative Effect|generative effects]]. The converse direction — limit preservation implying adjointness — needs the [[Adjoint Functor Theorem]].

````tabs
tab: Lean
```lean
#check CategoryTheory.Adjunction.rightAdjointPreservesLimits    -- PreservesLimitsOfSize G
#check CategoryTheory.Adjunction.leftAdjointPreservesColimits
#check CategoryTheory.Limits.preservesLimitsOfNatIso
```
tab: Haskell
```haskell
-- (- , a) is a left adjoint, so it preserves sums: distributivity
distr :: (Either b c, a) -> Either (b, a) (c, a)
distr (Left b, a)  = Left (b, a)
distr (Right c, a) = Right (c, a)
undistr :: Either (b, a) (c, a) -> (Either b c, a)
undistr (Left (b, a))  = (Left b, a)
undistr (Right (c, a)) = (Right c, a)
```
````
