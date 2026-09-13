#solution

Solutions to the exercises of DaoFP, Chapter 18: [[DaoFP Chapter 18 Exercises]]. Index: [[Map of Content]].

## Solution 18.1.1

#proof — [[DaoFP Chapter 18 Exercises#Exercise 18.1.1|Exercise 18.1.1]]

With $a = b = *$: $\int_{F : [\mathcal{M}, \mathbf{Set}]} \mathbf{Set}(F *, F *) \cong \mathcal{M}(*, *)$. By Yoneda, $F * \cong [\mathcal{M}, \mathbf{Set}](\mathcal{M}(*, -), F)$ where $\mathcal{M}(*, -)$ is the *regular representation* — the monoid acting on itself by post-composition. The end becomes $\int_F \mathbf{Set}(\mathrm{Nat}(R, F), \mathrm{Nat}(R, F))$ with $R$ the regular representation, which by the Yoneda corollary in $[\mathcal{M}, \mathbf{Set}]$ is $\mathrm{Nat}(R, R) \cong \mathcal{M}(*, *)$: the equivariant endomaps of the regular representation are exactly right multiplications by monoid elements. So the monoid is recovered from its category of $M$-sets.

> Sources: DaoFP Exercise 18.1.1.

## Solution 18.2.1

#program — [[DaoFP Chapter 18 Exercises#Exercise 18.2.1|Exercise 18.2.1]]

```haskell
newtype Adapter a b s t = Ad (s -> a, b -> t)
instance Profunctor (Adapter a b) where
  dimap f g (Ad (h, k)) = Ad (h . f, g . k)

fromIsoP :: IsoP s t a b -> (s -> a, b -> t)
fromIsoP pp = let Ad p = pp (Ad (id, id)) in p
```
`Adapter a b` is a profunctor in `s t`; feeding the identity adapter `Ad (id, id) :: Adapter a b a b` to the polymorphic function yields `Adapter a b s t`, whose contents are the sought pair.

> Sources: DaoFP Exercise 18.2.1.

## Solution 18.4.1

#annotation — [[DaoFP Chapter 18 Exercises#Exercise 18.4.1|Exercise 18.4.1]]

If $\mathcal{D} = \mathbf{1}$, the second hom-set $\mathcal{D}(m \bullet b, t)$ is a singleton and the optic reduces to $\int^{m} \mathcal{C}(s, m \times a)$; by co-Yoneda-style reasoning this is just $\mathcal{C}(s, a)$ up to the residue — a *getter* (the $b, t$ side is trivial). With the first category $\mathcal{C}^{\mathrm{op}} \times \mathcal{C}$ and the second terminal, $s$ and $a$ are pairs and the optic is $\int^{m} (\mathcal{C}^{\mathrm{op}} \times \mathcal{C})(\langle s, t\rangle, m \bullet \langle a, b\rangle) = \int^m \mathcal{C}(m \times a, s) \times \mathcal{C}(t, m \times b)$ — the existential lens with all arrows reversed, i.e. a lens in the opposite category.

> Sources: DaoFP Exercise 18.4.1.
