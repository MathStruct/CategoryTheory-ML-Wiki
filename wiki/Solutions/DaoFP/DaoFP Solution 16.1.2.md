#solution #program

**Solution to [[DaoFP Exercise 16.1.2|Exercise 16.1.2]].**

```haskell
data BiStream a = BStr [a] [a] deriving Functor
instance Comonad BiStream where
  extract (BStr _ (a : _)) = a
  duplicate s = BStr (tail (iterate left s)) (iterate right s)
    where left  (BStr (p : ps) fs) = BStr ps (p : fs)      -- move the cursor to the past
          right (BStr ps (f : fs)) = BStr (f : ps) fs      -- move the cursor to the future
```
`duplicate` places at each position the whole stream re-centred there.

> Sources: DaoFP Exercise 16.1.2.
