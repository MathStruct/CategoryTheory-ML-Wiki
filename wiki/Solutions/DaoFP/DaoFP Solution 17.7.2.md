#solution #program

**Solution to [[DaoFP Exercise 17.7.2|Exercise 17.7.2]].**

```haskell
assoc :: Day f (Day g h) x -> Day (Day f g) h x
assoc (Day abx fa (Day cdb gc hd)) =
  Day (\((a, c), d) -> abx (a, cdb (c, d))) (Day (,) fa gc) hd
```
The new inner existential type is the pair `(a, c)`; the combining function re-associates.

> Sources: DaoFP Exercise 17.7.2.
