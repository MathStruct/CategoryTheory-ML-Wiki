#exercise #solution #program

**Exercise 8.3.3.** Show `Constant c` is a `Functor` (in its second argument).

## Solution

```haskell
data Constant c a = Constant c
instance Functor (Constant c) where
  fmap _ (Constant c) = Constant c
```
It is the [[Constant Functor]] $\Delta_c$: every arrow goes to the identity on $c$.
