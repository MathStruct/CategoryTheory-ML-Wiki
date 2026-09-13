#solution #program

**Solution to [[DaoFP Exercise 19.3.1|Exercise 19.3.1]].**

```haskell
instance Functor (Ran p f) where
  fmap g (Ran h) = Ran (\k -> h (k . g))     -- k :: b' -> p e, g :: b -> b'
```

> Sources: DaoFP Exercise 19.3.1.
