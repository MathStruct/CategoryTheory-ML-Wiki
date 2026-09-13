#solution

Solutions to the exercises of DaoFP, Chapter 12: [[DaoFP Chapter 12 Exercises]]. Index: [[Map of Content]].

## Solution 12.2.1

#proof — [[DaoFP Chapter 12 Exercises#Exercise 12.2.1|Exercise 12.2.1]]

A morphism must satisfy `show . eval = pretty . fmap show`. On the node `PlusF 2 3 :: ExprF Int`: the left side gives `show (2 + 3) = "5"`, the right side gives `pretty (PlusF "2" "3") = "2 + 3"`. They differ, so the square does not commute.

> Sources: DaoFP Exercise 12.2.1.

## Solution 12.2.2

#proof — [[DaoFP Chapter 12 Exercises#Exercise 12.2.2|Exercise 12.2.2]]

Check `log . mulAlg = addAlg . fmap log` on both constructors. `Num x`: `log (mulAlg (Num x)) = log x` and `addAlg (fmap log (Num x)) = addAlg (Num x) = log x`. `Op x y`: `log (x * y) = log x + log y = addAlg (Op (log x) (log y))`. The square commutes (up to floating-point rounding), so `log` is a morphism $(\mathtt{Float}, \mathtt{mulAlg}) \to (\mathtt{Float}, \mathtt{addAlg})$ — the classical fact that the logarithm turns products into sums.

> Sources: DaoFP Exercise 12.2.2.

## Solution 12.5.1

#program — [[DaoFP Chapter 12 Exercises#Exercise 12.5.1|Exercise 12.5.1]]

```haskell
{-# LANGUAGE RankNTypes, ScopedTypeVariables #-}
data ListF a x = NilF | ConsF a x
newtype Mu f = Mu (forall a. (f a -> a) -> a)
cataMu :: (f a -> a) -> Mu f -> a
cataMu alg (Mu h) = h alg

fromList :: forall a. [a] -> Mu (ListF a)
fromList as = Mu h
  where h :: forall x. (ListF a x -> x) -> x
        h alg = go as where go [] = alg NilF
                            go (n : ns) = alg (ConsF n (go ns))

sumAlg :: ListF Int Int -> Int
sumAlg NilF = 0
sumAlg (ConsF n s) = n + s

test :: Bool
test = cataMu sumAlg (fromList [1 .. 10]) == 55
```

> Sources: DaoFP Exercise 12.5.1.
