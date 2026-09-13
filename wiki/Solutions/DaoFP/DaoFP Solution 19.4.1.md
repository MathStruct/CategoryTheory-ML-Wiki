#solution #program

**Solution to [[DaoFP Exercise 19.4.1|Exercise 19.4.1]].**

```haskell
instance Functor (Lan p f) where
  fmap g (Lan pe_b fe) = Lan (g . pe_b) fe
```

> Sources: DaoFP Exercise 19.4.1.
