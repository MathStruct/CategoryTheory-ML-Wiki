#solution #program

**Solution to [[DaoFP Exercise 19.3.3|Exercise 19.3.3]].**

```haskell
instance Functor (Codensity f) where
  fmap g (C h) = C (\k -> h (k . g))
```

> Sources: DaoFP Exercise 19.3.3.
