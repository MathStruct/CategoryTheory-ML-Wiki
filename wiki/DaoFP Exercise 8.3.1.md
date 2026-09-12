#exercise #solution #program

**Exercise 8.3.1.** Show `data WithInt a = WithInt a Int` is a [[Functor]].

## Solution

```haskell
instance Functor WithInt where
  fmap f (WithInt a n) = WithInt (f a) n
```
`fmap id = id` and `fmap (g . f) = fmap g . fmap f` hold since `n` is untouched.
