#solution #program

**Solution to [[DaoFP Exercise 19.3.4|Exercise 19.3.4]].**

```haskell
instance Applicative (Codensity f) where
  pure x = C (\k -> k x)
  C hf <*> C hx = C (\k -> hf (\g -> hx (k . g)))
```
Run the function-producing computation with a continuation that runs the argument computation and feeds `k . g` to it.

> Sources: DaoFP Exercise 19.3.4.
