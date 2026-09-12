#exercise #solution #program

**Exercise 14.5.2.** Rewrite `pairs` ([[List Monad]]) using bind operators and lambdas.

## Solution

```haskell
pairs :: [a] -> [b] -> [(a, b)]
pairs as bs = as >>= \a -> bs >>= \b -> return (a, b)
-- = concat (fmap (\a -> fmap (\b -> (a, b)) bs) as)
```

> Sources: DaoFP Exercise 14.5.2.
