#exercise #solution #program

**Exercise 14.8.1.** Implement conversions between the rose tree `data Rose a = Leaf a | Rose [Rose a]` and `FreeMonad [] a` ([[Free Monad]]).

## Solution

```haskell
toFree :: Rose a -> FreeMonad [] a
toFree (Leaf a)  = Pure a
toFree (Rose rs) = Free (fmap toFree rs)

fromFree :: FreeMonad [] a -> Rose a
fromFree (Pure a)   = Leaf a
fromFree (Free frs) = Rose (fmap fromFree frs)
```
`Pure` ↔ `Leaf`, `Free` ↔ `Rose`: the free monad on the list functor *is* the type of rose trees.

> Sources: DaoFP Exercise 14.8.1.
