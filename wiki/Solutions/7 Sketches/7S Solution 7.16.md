#solution #example

**Solution to [[7S Exercise 7.16|Exercise 7.16]].**

1. $\mathsf{false}$: $-5 \notin \mathbb{N}$. 2. $\mathsf{true}$: $0 \in \mathbb{N}$.

````tabs
tab: Haskell
```haskell
charN :: Integer -> Bool
charN = (>= 0)          -- charN (-5) == False, charN 0 == True
```
````

> Sources: 7 Sketches, Exercise 7.16 and Solution A.7.
