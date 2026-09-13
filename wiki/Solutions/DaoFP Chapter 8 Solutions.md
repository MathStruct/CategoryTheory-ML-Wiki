#solution

Solutions to the exercises of DaoFP, Chapter 8: [[DaoFP Chapter 8 Exercises]]. Index: [[Map of Content]].

## Solution 8.2.1

[[DaoFP Chapter 8 Exercises#Exercise 8.2.1|Exercise 8.2.1]]

It picks two objects $F(a), F(b)$ and an arrow $F(f) : F(a) \to F(b)$ (identities go to identities). Functors $\underline{\mathbf{2}} \to \mathcal{C}$ *are* the arrows of $\mathcal{C}$.

> Sources: DaoFP Exercise 8.2.1.

## Solution 8.2.2

[[DaoFP Chapter 8 Exercises#Exercise 8.2.2|Exercise 8.2.2]]

$F(g) \circ F(f) = F(g \circ f) = F(\mathrm{id}_a) = \mathrm{id}_{F a}$ and likewise $F(f) \circ F(g) = \mathrm{id}_{Fb}$, so $F(f)$ is an [[Isomorphism]] with inverse $F(g)$.

> Sources: DaoFP Exercise 8.2.2.

## Solution 8.3.1

#program — [[DaoFP Chapter 8 Exercises#Exercise 8.3.1|Exercise 8.3.1]]

```haskell
instance Functor WithInt where
  fmap f (WithInt a n) = WithInt (f a) n
```
`fmap id = id` and `fmap (g . f) = fmap g . fmap f` hold since `n` is untouched.

> Sources: DaoFP Exercise 8.3.1.

## Solution 8.3.2

#program — [[DaoFP Chapter 8 Exercises#Exercise 8.3.2|Exercise 8.3.2]]

```haskell
newtype Identity a = Identity a
instance Functor Identity where
  fmap f (Identity a) = Identity (f a)
```

> Sources: DaoFP Exercise 8.3.2.

## Solution 8.3.3

#program — [[DaoFP Chapter 8 Exercises#Exercise 8.3.3|Exercise 8.3.3]]

```haskell
data Constant c a = Constant c
instance Functor (Constant c) where
  fmap _ (Constant c) = Constant c
```
It is the [[Constant Functor]] $\Delta_c$: every arrow goes to the identity on $c$.

> Sources: DaoFP Exercise 8.3.3.

## Solution 8.3.4

#program — [[DaoFP Chapter 8 Exercises#Exercise 8.3.4|Exercise 8.3.4]]

```haskell
instance Bifunctor MoreThanA where
  bimap g h (More a mb) = More (g a) (fmap h mb)
```

> Sources: DaoFP Exercise 8.3.4.

## Solution 8.5.1

#program — [[DaoFP Chapter 8 Exercises#Exercise 8.5.1|Exercise 8.5.1]]

The composite is contravariant:
```haskell
instance (Functor g, Contravariant f) => Contravariant (Compose g f) where
  contramap h (Compose gfa) = Compose (fmap (contramap h) gfa)
```
See [[Contravariant Functor]].

> Sources: DaoFP Exercise 8.5.1.
