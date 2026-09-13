#solution #program

**Solution to [[DaoFP Exercise 14.8.1|Exercise 14.8.1]].**

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
