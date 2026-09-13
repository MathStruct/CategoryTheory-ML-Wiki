#solution #program

**Solution to [[DaoFP Exercise 14.5.1|Exercise 14.5.1]].**

```haskell
ap :: Monad m => m (a -> b) -> m a -> m b
ap fs as = do
  f <- fs
  a <- as
  return (f a)
```
This is the splat of the [[Applicative Functor]] derived from any monad.

> Sources: DaoFP Exercise 14.5.1.
