#solution #program

**Solution to [[DaoFP Exercise 9.8.4|Exercise 9.8.4]].**

Yes, by the [[Initial Object]] (`Void`): $\mathcal{C}(0, x) \cong 1$ — "the logarithm of 1 is 0".
```haskell
instance Representable Unit where
  type Key Unit = Void
  tabulate _ = U
  index U = absurd
```

> Sources: DaoFP Exercise 9.8.4.
