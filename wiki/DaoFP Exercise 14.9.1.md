#exercise #solution #program

**Exercise 14.9.1.** Implement the `Monoidal` instance for the list functor ([[Monoidal Functor]], [[Applicative Functor]]).

## Solution

```haskell
instance Monoidal [] where
  unit = [()]
  as >*< bs = [ (a, b) | a <- as, b <- bs ]     -- cartesian product
```
(The zip version `zip as bs` with `unit = repeat ()` is another lax monoidal structure.)

> Sources: DaoFP Exercise 14.9.1.
