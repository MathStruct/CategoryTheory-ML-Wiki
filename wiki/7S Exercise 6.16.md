#exercise #solution #example

**Exercise 6.16.** Let $T = \{a, \dots, z\}$, $A = \{\mathrm{apple}, \mathrm{banana}, \mathrm{pear}, \mathrm{cherry}, \mathrm{orange}\}$, $B = \{\mathrm{apple}, \mathrm{tomato}, \mathrm{mango}\}$. Let $f : A \to T$ send each element to its first letter and $g : B \to T$ send each element to its last letter. Write down the copairing $[f, g] : A \sqcup B \to T$ on all eight elements.

## Solution

| $A \sqcup B$ | apple$_1$ | banana$_1$ | pear$_1$ | cherry$_1$ | orange$_1$ | apple$_2$ | tomato$_2$ | mango$_2$ |
|---|---|---|---|---|---|---|---|---|
| $[f, g]$ | a | b | p | c | o | e | o | o |

The [[Coproduct]] in $\mathbf{Set}$ is the disjoint union, so the two apples are distinct elements with different images.

```tabs
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
```

> Sources: 7 Sketches, Exercise 6.16 and Solution A.6.
