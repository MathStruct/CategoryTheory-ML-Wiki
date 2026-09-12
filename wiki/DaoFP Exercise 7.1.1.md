#exercise #solution #program

**Exercise 7.1.1.** Implement a function turning a `Nat` into an `Int` using the recursor ([[Natural Numbers Object]]).

## Solution

```haskell
natToInt :: Nat -> Int
natToInt = rec 0 (+ 1)         -- init = 0, step = successor on Int
```

> Sources: DaoFP Exercise 7.1.1.
