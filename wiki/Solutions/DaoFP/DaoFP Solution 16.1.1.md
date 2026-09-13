#solution #program

**Solution to [[DaoFP Exercise 16.1.1|Exercise 16.1.1]].**

```haskell
duplicate :: Comonad w => w a -> w (w a)
duplicate = extend id
extend :: Comonad w => (w a -> b) -> w a -> w b
extend f = fmap f . duplicate
```
Dual to `join = (>>= id)` and `ma >>= k = join (fmap k ma)`.

> Sources: DaoFP Exercise 16.1.1.
