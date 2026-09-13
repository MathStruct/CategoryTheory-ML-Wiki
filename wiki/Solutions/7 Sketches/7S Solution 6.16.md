#solution #example

**Solution to [[7S Exercise 6.16|Exercise 6.16]].**

| $A \sqcup B$ | apple$_1$ | banana$_1$ | pear$_1$ | cherry$_1$ | orange$_1$ | apple$_2$ | tomato$_2$ | mango$_2$ |
|---|---|---|---|---|---|---|---|---|
| $[f, g]$ | a | b | p | c | o | e | o | o |

The [[Coproduct]] in $\mathbf{Set}$ is the disjoint union, so the two apples are distinct elements with different images.

````tabs
tab: Julia
```julia
using Catlab
A = FinSet(5); B = FinSet(3); T = FinSet(26)            # letters as 1..26
letter(c) = Int(c) - Int('a') + 1
f = FinFunction(letter.(['a','b','p','c','o']), 26)     # first letters
g = FinFunction(letter.(['e','o','o']), 26)             # last letters
cp = coproduct(A, B)
h = copair(cp, f, g)                                    # [f, g] : 8 → 26
collect(h)                                              # [1, 2, 16, 3, 15, 5, 15, 15]
```
tab: Haskell
```haskell
copair :: (a -> t) -> (b -> t) -> Either a b -> t
copair = either
-- either (head) (last) :: Either String String -> Char
```
````

> Sources: 7 Sketches, Exercise 6.16 and Solution A.6.
