#solution #program

**Solution to [[DaoFP Exercise 17.2.3|Exercise 17.2.3]].**

```haskell
newtype CoEndCompose p q a b = CoEndCompose (Coend (ProPair q p a b))
instance (Profunctor p, Profunctor q) => Profunctor (CoEndCompose p q) where
  dimap l r (CoEndCompose (Coend (ProPair (qax, pxb)))) =
    CoEndCompose (Coend (ProPair (dimap l id qax, dimap id r pxb)))
```
Extending on the left acts on `q`, extending on the right on `p`; the hidden middle type `x` is untouched — the same as for `Procompose`.

> Sources: DaoFP Exercise 17.2.3.
