#solution #program

**Solution to [[DaoFP Exercise 19.4.2|Exercise 19.4.2]].**

```haskell
instance Functor (Density f) where
  fmap g (D fd_c fd) = D (g . fd_c) fd
instance Comonad (Density f) where
  extract (D fd_c fd) = fd_c fd                    -- apply the function to the hidden value
  duplicate (D fd_c fd) = D (D fd_c) fd            -- keep the same hidden f d, delay the application
```
This is the dual of the [[Codensity Monad]]. With `f = Identity`, `Density Identity c ≅ exists d. (d -> c, d) ≅ c` by co-Yoneda, so it collapses to the identity comonad.

> Sources: DaoFP Exercise 19.4.2.
