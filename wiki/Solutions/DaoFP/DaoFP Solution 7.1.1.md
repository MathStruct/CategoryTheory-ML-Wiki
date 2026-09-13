#solution #program

**Solution to [[DaoFP Exercise 7.1.1|Exercise 7.1.1]].**

```haskell
natToInt :: Nat -> Int
natToInt = rec 0 (+ 1)         -- init = 0, step = successor on Int
```

> Sources: DaoFP Exercise 7.1.1.
