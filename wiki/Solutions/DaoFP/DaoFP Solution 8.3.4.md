#solution #program

**Solution to [[DaoFP Exercise 8.3.4|Exercise 8.3.4]].**

```haskell
instance Bifunctor MoreThanA where
  bimap g h (More a mb) = More (g a) (fmap h mb)
```

> Sources: DaoFP Exercise 8.3.4.
