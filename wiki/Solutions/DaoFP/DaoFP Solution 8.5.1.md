#solution #program

**Solution to [[DaoFP Exercise 8.5.1|Exercise 8.5.1]].**

The composite is contravariant:
```haskell
instance (Functor g, Contravariant f) => Contravariant (Compose g f) where
  contramap h (Compose gfa) = Compose (fmap (contramap h) gfa)
```
See [[Contravariant Functor]].

> Sources: DaoFP Exercise 8.5.1.
