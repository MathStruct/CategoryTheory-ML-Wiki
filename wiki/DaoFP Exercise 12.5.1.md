#exercise #solution #program

**Exercise 12.5.1.** Write a test that converts a list of integers to the `Mu` form and sums it with `cataMu` ([[Initial Algebra]]).

## Solution

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
