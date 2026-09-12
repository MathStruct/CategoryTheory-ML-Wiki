#exercise #solution #program

**Exercise 8.5.1.** Define the composition of a `Functor` after a `Contravariant` functor.

## Solution

The composite is contravariant:
```haskell
instance (Functor g, Contravariant f) => Contravariant (Compose g f) where
  contramap h (Compose gfa) = Compose (fmap (contramap h) gfa)
```
See [[Contravariant Functor]].
