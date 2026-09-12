#exercise #solution #program

**Exercise 9.3.3.** Same for `reverse` after `safeHead`: `reverse . fmap safeHead` vs `fmap safeHead . reverse` of type `[[a]] -> [Maybe a]`.

## Solution

Both send `[[1,2],[],[3]]` to `[Just 3, Nothing, Just 1]`; equal by naturality of `reverse`.
