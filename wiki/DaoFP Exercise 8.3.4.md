#exercise #solution #program

**Exercise 8.3.4.** Show `data MoreThanA a b = More a (Maybe b)` is a [[Bifunctor]].

## Solution

```haskell
instance Bifunctor MoreThanA where
  bimap g h (More a mb) = More (g a) (fmap h mb)
```
