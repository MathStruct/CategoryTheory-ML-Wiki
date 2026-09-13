#solution #program

**Solution to [[DaoFP Exercise 19.1.1|Exercise 19.1.1]].**

From $[G, H]_{\mathrm{Day}}\, a = \int_b [G b, H(a \times b)]$:
```haskell
{-# LANGUAGE RankNTypes #-}
type DayHom g h a = forall b. g b -> h (a, b)
```

> Sources: DaoFP Exercise 19.1.1.
