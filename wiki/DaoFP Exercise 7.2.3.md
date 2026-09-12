#exercise #solution #program

**Exercise 7.2.3.** Implement a function extracting the third element of a [[List]], if it is long enough, with result type `Maybe a`.

## Solution

```haskell
third :: [a] -> Maybe a
third (_ : _ : x : _) = Just x
third _               = Nothing
-- pattern matching is the idiomatic elimination rule; as a fold one would thread a counter
-- through the accumulator, which is far less readable.
```

> Sources: DaoFP Exercise 7.2.3.
