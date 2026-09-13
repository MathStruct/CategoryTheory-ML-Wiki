#solution #program

**Solution to [[DaoFP Exercise 17.7.1|Exercise 17.7.1]].**

```haskell
instance Functor (Day f g) where
  fmap h (Day abx fa gb) = Day (h . abx) fa gb
```

> Sources: DaoFP Exercise 17.7.1.
