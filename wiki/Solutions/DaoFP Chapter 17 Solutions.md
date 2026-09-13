#solution

Solutions to the exercises of DaoFP, Chapter 17: [[DaoFP Chapter 17 Exercises]]. Index: [[Map of Content]].

## Solution 17.1.1

#proof — [[DaoFP Chapter 17 Exercises#Exercise 17.1.1|Exercise 17.1.1]]

Send every object of $\mathcal{C}$ to $0$ and every object of $\mathcal{D}$ to $1$; send morphisms of $\mathcal{C}$ to $\mathrm{id}_0$, morphisms of $\mathcal{D}$ to $\mathrm{id}_1$, and heteromorphisms (elements of $P\langle c, d\rangle$) to the unique arrow $0 \to 1$. Composition is preserved: composing a heteromorphism with a $\mathcal{C}$- or $\mathcal{D}$-morphism is again a heteromorphism, mapped to $0 \to 1 = \mathrm{id}_1 \circ (0 \to 1) = (0 \to 1) \circ \mathrm{id}_0$; there are no composable pairs of heteromorphisms since none go from $\mathcal{D}$ to $\mathcal{C}$.

> Sources: DaoFP Exercise 17.1.1.

## Solution 17.1.2

#proof — [[DaoFP Chapter 17 Exercises#Exercise 17.1.2|Exercise 17.1.2]]

Let $\mathcal{C} = F^{-1}(0)$ and $\mathcal{D} = F^{-1}(1)$ be the full subcategories on the objects sent to $0$ and $1$. Since $\mathbf{2}$ has no arrow $1 \to 0$, there are no morphisms from $\mathcal{D}$-objects to $\mathcal{C}$-objects. Define $P\langle c, d\rangle := \mathcal{E}(c, d)$; it is a [[Profunctor]] $\mathcal{C}^{\mathrm{op}} \times \mathcal{D} \to \mathbf{Set}$ by pre- and post-composition. Then $\mathcal{E}$ is exactly the collage of $P$: objects the disjoint union, hom-sets as in $\mathcal{C}$, $\mathcal{D}$, or $P$, and composition inherited from $\mathcal{E}$.

> Sources: DaoFP Exercise 17.1.2.

## Solution 17.2.1

#proof — [[DaoFP Chapter 17 Exercises#Exercise 17.2.1|Exercise 17.2.1]]

With $Q = \Delta_d$ constant, $Q\langle d', d'\rangle = d$ for all $d'$ and $Q\langle \mathrm{id}, g\rangle = Q\langle g, \mathrm{id}\rangle = \mathrm{id}_d$. The first diamond, for $f : c \to c'$, reads $\alpha_{c'} \circ P\langle \mathrm{id}, f\rangle = \alpha_c \circ P\langle f, \mathrm{id}\rangle$ on $P\langle c', c\rangle$ — exactly the cowedge condition. The second diamond, for $g : d \to d'$, reads $\mathrm{id}_d \circ \alpha_{c} = \mathrm{id}_d \circ \alpha_{c}$, trivially true (the components do not depend on the second index).

> Sources: DaoFP Exercise 17.2.1.

## Solution 17.2.2

#program — [[DaoFP Chapter 17 Exercises#Exercise 17.2.2|Exercise 17.2.2]]

```haskell
newtype ProPair q p a b x y = ProPair (q a y, p x b)
instance (Profunctor p, Profunctor q) => Profunctor (ProPair q p a b) where
  dimap f g (ProPair (qay, pxb)) = ProPair (dimap id g qay, dimap f id pxb)
```
`x` is contravariant (it is the source of `p x b`), `y` covariant (the target of `q a y`).

> Sources: DaoFP Exercise 17.2.2.

## Solution 17.2.3

#program — [[DaoFP Chapter 17 Exercises#Exercise 17.2.3|Exercise 17.2.3]]

```haskell
newtype CoEndCompose p q a b = CoEndCompose (Coend (ProPair q p a b))
instance (Profunctor p, Profunctor q) => Profunctor (CoEndCompose p q) where
  dimap l r (CoEndCompose (Coend (ProPair (qax, pxb)))) =
    CoEndCompose (Coend (ProPair (dimap l id qax, dimap id r pxb)))
```
Extending on the left acts on `q`, extending on the right on `p`; the hidden middle type `x` is untouched — the same as for `Procompose`.

> Sources: DaoFP Exercise 17.2.3.

## Solution 17.3.1

#proof — [[DaoFP Chapter 17 Exercises#Exercise 17.3.1|Exercise 17.3.1]]

Let $F : \mathbf{2} \to \mathcal{C}$ pick $a, b$, and $P\langle x, y\rangle := F y$. A wedge is an object $d$ with $\pi_1 : d \to a$, $\pi_2 : d \to b$; since $\mathbf{2}$ has only identity arrows the wedge condition is vacuous. The universal wedge is therefore an object with two projections through which every such pair factors uniquely — the product $a \times b$. So $\int_{x : \mathbf{2}} F x = a \times b$.

> Sources: DaoFP Exercise 17.3.1.

## Solution 17.6.1

#proof — [[DaoFP Chapter 17 Exercises#Exercise 17.6.1|Exercise 17.6.1]]

For an arbitrary set $S$: $\mathbf{Set}(\int^x \mathcal{C}(a, x) \times G x, S) \cong \int_x \mathbf{Set}(\mathcal{C}(a, x) \times G x, S) \cong \int_x \mathbf{Set}(\mathcal{C}(a, x), S^{G x})$. The functor $x \mapsto S^{G x}$ is covariant (contravariant twice), so the covariant ninja Yoneda lemma gives $S^{G a} \cong \mathbf{Set}(G a, S)$. By the Yoneda corollary (objects with isomorphic mapping-outs are isomorphic), $\int^x \mathcal{C}(a, x) \times G x \cong G a$.

> Sources: DaoFP Exercise 17.6.1.

## Solution 17.7.1

#program — [[DaoFP Chapter 17 Exercises#Exercise 17.7.1|Exercise 17.7.1]]

```haskell
instance Functor (Day f g) where
  fmap h (Day abx fa gb) = Day (h . abx) fa gb
```

> Sources: DaoFP Exercise 17.7.1.

## Solution 17.7.2

#program — [[DaoFP Chapter 17 Exercises#Exercise 17.7.2|Exercise 17.7.2]]

```haskell
assoc :: Day f (Day g h) x -> Day (Day f g) h x
assoc (Day abx fa (Day cdb gc hd)) =
  Day (\((a, c), d) -> abx (a, cdb (c, d))) (Day (,) fa gc) hd
```
The new inner existential type is the pair `(a, c)`; the combining function re-associates.

> Sources: DaoFP Exercise 17.7.2.

## Solution 17.7.3

#program — [[DaoFP Chapter 17 Exercises#Exercise 17.7.3|Exercise 17.7.3]]

```haskell
instance Functor f => Functor (FreeA f) where
  fmap h (DoneA x)          = DoneA (h x)
  fmap h (MoreA abx fa frb) = MoreA (h . abx) fa frb
```
Only the combining function at the head changes; the tail is untouched.

> Sources: DaoFP Exercise 17.7.3.

## Solution 17.9.1

#proof — [[DaoFP Chapter 17 Exercises#Exercise 17.9.1|Exercise 17.9.1]]

Given $f : x' \to x$ and $g : y \to y'$, map $(l, r) \mapsto ((g \times \mathrm{id}_a) \circ l,\ r \circ (f \times \mathrm{id}_b))$: the first factor is covariant in $y$ (post-composition), the second contravariant in $x$ (pre-composition). Functoriality follows from that of composition and of $\times$. So it is a functor $\mathcal{C}^{\mathrm{op}} \times \mathcal{C} \to \mathbf{Set}$ in $\langle x, y\rangle$ with $\langle s, t\rangle, \langle a, b\rangle$ fixed, and its coend is the existential lens.

> Sources: DaoFP Exercise 17.9.1.
