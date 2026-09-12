#exercise #solution #program

**Exercise 9.5.2.** Check that `(h q') . q == q'` for `q' x = testBit x 0` (with `q n = even n`, `h q' True = q' 0`, `h q' False = q' 1`).

## Solution

`q' x` is `True` iff `x` is odd; `h q' (q x)` returns `q' 0 = False` when `x` is even and `q' 1 = True` when odd. Equal for all `x`. See [[Coequalizer]].
