#solution #program

**Solution to [[DaoFP Exercise 14.9.1|Exercise 14.9.1]].**

```haskell
instance Monoidal [] where
  unit = [()]
  as >*< bs = [ (a, b) | a <- as, b <- bs ]     -- cartesian product
```
(The zip version `zip as bs` with `unit = repeat ()` is another lax monoidal structure.)

> Sources: DaoFP Exercise 14.9.1.
