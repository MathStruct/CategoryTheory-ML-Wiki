#exercise #solution #program

**Exercise 9.8.4.** Is the constant functor to the terminal object representable? Implement `Representable` for `data Unit a = U`.

## Solution

Yes, by the [[Initial Object]] (`Void`): $\mathcal{C}(0, x) \cong 1$ — "the logarithm of 1 is 0".
```haskell
instance Representable Unit where
  type Key Unit = Void
  tabulate _ = U
  index U = absurd
```
