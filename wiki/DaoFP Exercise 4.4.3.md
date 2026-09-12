#exercise #solution #program

**Exercise 4.4.3.** Implement the function witnessing `Either a b ≅ Either b a`; note it is its own inverse ([[Sum Type]]).

## Solution

```haskell
swapE :: Either a b -> Either b a
swapE (Left a)  = Right a
swapE (Right b) = Left b
-- swapE . swapE = id, so swapE is an involution (point-free: swapE = either Right Left)
```

> Sources: DaoFP Exercise 4.4.3.
