#exercise #solution #program

**Exercise 9.3.2.** Implement two versions of the horizontal composition of `safeHead` after `reverse` and compare.

## Solution

`safeHead . fmap reverse` and `fmap reverse . safeHead`, both of type `[[a]] -> Maybe [a]`; e.g. on `[[1,2],[3]]` both give `Just [2,1]`, on `[]` both give `Nothing`. Equal by naturality of `safeHead`.
