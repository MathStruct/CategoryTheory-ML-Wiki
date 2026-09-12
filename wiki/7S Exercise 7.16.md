#exercise #solution #example

**Exercise 7.16.** Let $m : \mathbb{N} \hookrightarrow \mathbb{Z}$ be the inclusion, with characteristic function $\ulcorner m \urcorner : \mathbb{Z} \to \mathbb{B}$ ([[Subobject Classifier]]). 1. What is $\ulcorner m \urcorner(-5)$? 2. What is $\ulcorner m \urcorner(0)$?

## Solution

1. $\mathsf{false}$: $-5 \notin \mathbb{N}$. 2. $\mathsf{true}$: $0 \in \mathbb{N}$.

```tabs
tab: Haskell
```haskell
charN :: Integer -> Bool
charN = (>= 0)          -- charN (-5) == False, charN 0 == True
```
```

> Sources: 7 Sketches, Exercise 7.16 and Solution A.7.
