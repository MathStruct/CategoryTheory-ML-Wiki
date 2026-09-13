#solution #program

**Solution to [[DaoFP Exercise 8.3.3|Exercise 8.3.3]].**

```haskell
data Constant c a = Constant c
instance Functor (Constant c) where
  fmap _ (Constant c) = Constant c
```
It is the [[Constant Functor]] $\Delta_c$: every arrow goes to the identity on $c$.

> Sources: DaoFP Exercise 8.3.3.
