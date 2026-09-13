#solution #program

**Solution to [[DaoFP Exercise 19.1.2|Exercise 19.1.2]].**

```haskell
ltor :: (forall a. Day f g a -> h a) -> (forall a. f a -> DayHom g h a)
ltor nat fa = \gb -> nat (Day id fa gb)

rtol :: Functor h => (forall a. f a -> DayHom g h a) -> (forall a. Day f g a -> h a)
rtol nat (Day abx fa gb) = fmap abx (nat fa gb)
```
`ltor` packages `fa` and `gb` into a Day product with the identity combiner; `rtol` applies the curried natural transformation and then maps the combining function over the result.

> Sources: DaoFP Exercise 19.1.2.
