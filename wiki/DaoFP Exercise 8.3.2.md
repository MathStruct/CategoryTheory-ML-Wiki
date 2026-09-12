#exercise #solution #program

**Exercise 8.3.2.** Show `Identity` is a `Functor`.

## Solution

```haskell
newtype Identity a = Identity a
instance Functor Identity where
  fmap f (Identity a) = Identity (f a)
```
