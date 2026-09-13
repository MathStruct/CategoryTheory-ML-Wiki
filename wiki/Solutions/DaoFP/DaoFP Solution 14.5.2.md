#solution #program

**Solution to [[DaoFP Exercise 14.5.2|Exercise 14.5.2]].**

```haskell
pairs :: [a] -> [b] -> [(a, b)]
pairs as bs = as >>= \a -> bs >>= \b -> return (a, b)
-- = concat (fmap (\a -> fmap (\b -> (a, b)) bs) as)
```

> Sources: DaoFP Exercise 14.5.2.
