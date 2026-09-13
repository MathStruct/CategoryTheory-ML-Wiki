#solution #program

**Solution to [[DaoFP Exercise 8.3.2|Exercise 8.3.2]].**

```haskell
newtype Identity a = Identity a
instance Functor Identity where
  fmap f (Identity a) = Identity (f a)
```

> Sources: DaoFP Exercise 8.3.2.
