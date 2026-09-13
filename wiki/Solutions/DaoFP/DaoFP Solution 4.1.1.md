#solution #program

**Solution to [[DaoFP Exercise 4.1.1|Exercise 4.1.1]].**

A function out of `Bool` is a pair of elements of the target; the four pairs of booleans give four functions:

```haskell
idB, constTrue, constFalse, notB :: Bool -> Bool
idB b        = if b then True  else False    -- (True, False)
constTrue b  = if b then True  else True     -- (True, True)
constFalse b = if b then False else False    -- (False, False)
notB b       = if b then False else True     -- (False, True)
```

> Sources: DaoFP Exercise 4.1.1.
