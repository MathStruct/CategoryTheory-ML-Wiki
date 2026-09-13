#exercise

Exercises from DaoFP, Chapter 12. Solutions: [[DaoFP Chapter 12 Solutions]]. Index: [[Map of Content]].

## Exercise 12.2.1

Show that `show :: Int -> String` is not an [[Algebra of an Endofunctor|algebra morphism]] from `eval` to `pretty`.

> Sources: DaoFP Exercise 12.2.1.

**Solution:** [[DaoFP Chapter 12 Solutions#Solution 12.2.1|Solution 12.2.1]]

## Exercise 12.2.2

For `data FloatF x = Num Float | Op x x` with `addAlg (Num x) = log x; addAlg (Op x y) = x + y` and `mulAlg (Num x) = x; mulAlg (Op x y) = x * y`, argue that `log` is an [[Algebra of an Endofunctor|algebra morphism]] from `mulAlg` to `addAlg`.

> Sources: DaoFP Exercise 12.2.2.

**Solution:** [[DaoFP Chapter 12 Solutions#Solution 12.2.2|Solution 12.2.2]]

## Exercise 12.5.1

Write a test that converts a list of integers to the `Mu` form and sums it with `cataMu` ([[Initial Algebra]]).

> Sources: DaoFP Exercise 12.5.1.

**Solution:** [[DaoFP Chapter 12 Solutions#Solution 12.5.1|Solution 12.5.1]]
