#exercise #solution #proof

**Exercise 12.2.2.** For `data FloatF x = Num Float | Op x x` with `addAlg (Num x) = log x; addAlg (Op x y) = x + y` and `mulAlg (Num x) = x; mulAlg (Op x y) = x * y`, argue that `log` is an [[Algebra of an Endofunctor|algebra morphism]] from `mulAlg` to `addAlg`.

## Solution

Check `log . mulAlg = addAlg . fmap log` on both constructors. `Num x`: `log (mulAlg (Num x)) = log x` and `addAlg (fmap log (Num x)) = addAlg (Num x) = log x`. `Op x y`: `log (x * y) = log x + log y = addAlg (Op (log x) (log y))`. The square commutes (up to floating-point rounding), so `log` is a morphism $(\mathtt{Float}, \mathtt{mulAlg}) \to (\mathtt{Float}, \mathtt{addAlg})$ — the classical fact that the logarithm turns products into sums.

> Sources: DaoFP Exercise 12.2.2.
