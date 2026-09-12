#exercise #solution #program

**Exercise 11.1.1.** Implement `tailV` returning the tail of a non-zero-length vector. Try calling it with `emptyV` ([[Dependent Type]]).

## Solution

```haskell
tailV :: Vec ('S n) a -> Vec n a
tailV (VCons _ as) = as
-- tailV emptyV   -- type error: couldn't match 'Z with 'S n
```

The compiler rejects `tailV emptyV` because `Vec 'Z Int` does not unify with `Vec ('S n) a`; the pattern match is exhaustive since `VNil` cannot have type `Vec ('S n) a`.

> Sources: DaoFP Exercise 11.1.1.
