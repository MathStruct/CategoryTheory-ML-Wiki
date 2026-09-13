#solution #program

**Solution to [[DaoFP Exercise 7.2.3|Exercise 7.2.3]].**

```haskell
third :: [a] -> Maybe a
third (_ : _ : x : _) = Just x
third _               = Nothing
-- pattern matching is the idiomatic elimination rule; as a fold one would thread a counter
-- through the accumulator, which is far less readable.
```

> Sources: DaoFP Exercise 7.2.3.
