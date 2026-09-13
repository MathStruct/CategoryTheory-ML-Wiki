#exercise

**Exercise 12.2.2.** For `data FloatF x = Num Float | Op x x` with `addAlg (Num x) = log x; addAlg (Op x y) = x + y` and `mulAlg (Num x) = x; mulAlg (Op x y) = x * y`, argue that `log` is an [[Algebra of an Endofunctor|algebra morphism]] from `mulAlg` to `addAlg`.

**Solution:** [[DaoFP Solution 12.2.2]]

> Sources: DaoFP Exercise 12.2.2.
