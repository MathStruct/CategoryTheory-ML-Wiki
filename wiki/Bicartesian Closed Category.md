#definition #theorem #proof

A **bicartesian closed category** is a [[Cartesian Closed Category]] that also has all finite [[Coproduct|coproducts]] (an [[Initial Object]] $0$ and binary sums $a + b$). It is the categorical model of a typed programming language with product, sum and function types (and, via Curry–Howard, of intuitionistic propositional logic with $\wedge, \vee, \Rightarrow, \top, \bot$).

> Sources: DaoFP §6.3 ("Bicartesian Closed Categories", "Distributivity"), §10.2 ("Distributivity"), §10.7.

**Distributivity theorem.** In a bicartesian closed category, $(b + c) \times a \cong b \times a + c \times a$ and $0 \times a \cong 0$.

*Proof (DaoFP §10.2, via Yoneda).* For arbitrary $x$,
$$\mathcal{C}((b + c) \times a, x) \cong \mathcal{C}(b + c, x^a) \cong \mathcal{C}(b, x^a) \times \mathcal{C}(c, x^a) \cong \mathcal{C}(b \times a, x) \times \mathcal{C}(c \times a, x) \cong \mathcal{C}(b \times a + c \times a, x),$$
using the [[Currying|currying]] adjunction, the sum adjunction, currying backwards and the sum adjunction backwards; every step is natural in $x$, so by the [[Yoneda Lemma]] the objects are isomorphic. $\blacksquare$ *Shorter proof (§10.7):* $(- \times a)$ is a left adjoint, and left adjoints preserve colimits ([[Right Adjoints Preserve Limits]]); a coproduct is a colimit.

In Haskell the isomorphism is `(Either b c, a) ≅ Either (b, a) (c, a)`; DaoFP Chapter 6 gives the direct construction of one direction using the universal properties and notes the other direction needs the exponential. The identities $x^{a + b} \cong x^a \times x^b$, $(a \times b)^x \cong a^x \times b^x$ ("sum and product revisited") are further consequences.

````tabs
tab: Lean
```lean
-- Mathlib: distributivity of products over coproducts in a cartesian closed category
#check CategoryTheory.prodCoprodDistrib     -- (X ⨯ Y) ⨿ (X ⨯ Z) ≅ X ⨯ (Y ⨿ Z) in a CCC
```
tab: Haskell
```haskell
distribute :: (Either b c, a) -> Either (b, a) (c, a)
distribute (Left b, a)  = Left (b, a)
distribute (Right c, a) = Right (c, a)

undistribute :: Either (b, a) (c, a) -> (Either b c, a)
undistribute (Left (b, a))  = (Left b, a)
undistribute (Right (c, a)) = (Right c, a)
```
````
