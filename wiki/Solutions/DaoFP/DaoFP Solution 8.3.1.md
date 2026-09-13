#solution #program

**Solution to [[DaoFP Exercise 8.3.1|Exercise 8.3.1]].**

```haskell
instance Functor WithInt where
  fmap f (WithInt a n) = WithInt (f a) n
```
`fmap id = id` and `fmap (g . f) = fmap g . fmap f` hold since `n` is untouched.

> Sources: DaoFP Exercise 8.3.1.
