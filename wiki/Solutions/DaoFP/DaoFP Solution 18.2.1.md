#solution #program

**Solution to [[DaoFP Exercise 18.2.1|Exercise 18.2.1]].**

```haskell
newtype Adapter a b s t = Ad (s -> a, b -> t)
instance Profunctor (Adapter a b) where
  dimap f g (Ad (h, k)) = Ad (h . f, g . k)

fromIsoP :: IsoP s t a b -> (s -> a, b -> t)
fromIsoP pp = let Ad p = pp (Ad (id, id)) in p
```
`Adapter a b` is a profunctor in `s t`; feeding the identity adapter `Ad (id, id) :: Adapter a b a b` to the polymorphic function yields `Adapter a b s t`, whose contents are the sought pair.

> Sources: DaoFP Exercise 18.2.1.
