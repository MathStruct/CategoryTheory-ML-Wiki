#solution #proof

**Solution to [[DaoFP Exercise 12.2.2|Exercise 12.2.2]].**

Check `log . mulAlg = addAlg . fmap log` on both constructors. `Num x`: `log (mulAlg (Num x)) = log x` and `addAlg (fmap log (Num x)) = addAlg (Num x) = log x`. `Op x y`: `log (x * y) = log x + log y = addAlg (Op (log x) (log y))`. The square commutes (up to floating-point rounding), so `log` is a morphism $(\mathtt{Float}, \mathtt{mulAlg}) \to (\mathtt{Float}, \mathtt{addAlg})$ — the classical fact that the logarithm turns products into sums.

> Sources: DaoFP Exercise 12.2.2.
