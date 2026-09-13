#solution #program

**Solution to [[DaoFP Exercise 17.7.3|Exercise 17.7.3]].**

```haskell
instance Functor f => Functor (FreeA f) where
  fmap h (DoneA x)          = DoneA (h x)
  fmap h (MoreA abx fa frb) = MoreA (h . abx) fa frb
```
Only the combining function at the head changes; the tail is untouched.

> Sources: DaoFP Exercise 17.7.3.
