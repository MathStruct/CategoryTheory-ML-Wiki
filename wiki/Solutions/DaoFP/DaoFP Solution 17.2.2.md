#solution #program

**Solution to [[DaoFP Exercise 17.2.2|Exercise 17.2.2]].**

```haskell
newtype ProPair q p a b x y = ProPair (q a y, p x b)
instance (Profunctor p, Profunctor q) => Profunctor (ProPair q p a b) where
  dimap f g (ProPair (qay, pxb)) = ProPair (dimap id g qay, dimap f id pxb)
```
`x` is contravariant (it is the source of `p x b`), `y` covariant (the target of `q a y`).

> Sources: DaoFP Exercise 17.2.2.
