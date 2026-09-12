#exercise #solution #program

**Exercise 14.8.2.** Implement conversions between a non-empty binary tree and `FreeMonad Bin a` with `data Bin a = Bin a a` ([[Free Monad]]).

## Solution

```haskell
data Bin a = Bin a a deriving Functor
data BTree a = BLeaf a | BNode (BTree a) (BTree a)

toFreeB :: BTree a -> FreeMonad Bin a
toFreeB (BLeaf a)   = Pure a
toFreeB (BNode l r) = Free (Bin (toFreeB l) (toFreeB r))

fromFreeB :: FreeMonad Bin a -> BTree a
fromFreeB (Pure a)          = BLeaf a
fromFreeB (Free (Bin l r))  = BNode (fromFreeB l) (fromFreeB r)
```

> Sources: DaoFP Exercise 14.8.2.
