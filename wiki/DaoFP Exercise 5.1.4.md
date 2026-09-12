#exercise #solution #program

**Exercise 5.1.4.** Implement `maybeAB :: Either b (a, b) -> (Maybe a, b)`. Is it uniquely defined by its type?

## Solution

```haskell
maybeAB :: Either b (a, b) -> (Maybe a, b)
maybeAB (Left b)       = (Nothing, b)
maybeAB (Right (a, b)) = (Just a, b)
```

Not unique: `maybeAB (Right (a, b)) = (Nothing, b)` also type-checks, as would returning `Nothing` everywhere. Parametricity forces the `b` component (the only `b` available) but leaves the `Maybe a` component free.

> Sources: DaoFP Exercise 5.1.4.
