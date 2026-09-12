#exercise #solution #example #program

**Exercise 7.66.** In $\mathbf{Set}$ let $p : \mathbb{N} \times \mathbb{Z} \to \mathbb{B}$, $p(n, z) = [n \leq |z|]$. Find the sets where 1. $\forall(z : \mathbb{Z}).\, p(n, z)$; 2. $\exists(z : \mathbb{Z}).\, p(n, z)$; 3. $\forall(n : \mathbb{N}).\, p(n, z)$; 4. $\exists(n : \mathbb{N}).\, p(n, z)$ hold ([[Quantification]]).

## Solution

1. $\{0\}$ (only $0 \leq |z|$ for all $z$, since $z = 0$ must work). 2. All of $\mathbb{N}$ (take $z = n$). 3. $\varnothing$ (no $z$ exceeds every $n$). 4. All of $\mathbb{Z}$ (take $n = 0$).

```tabs
tab: Haskell
```haskell
p :: Integer -> Integer -> Bool
p n z = n <= abs z
-- on finite windows: [n | n <- [0..5], all (p n) [-9..9]] == [0]
--                    [z | z <- [-9..9], all (`p` z) [0..20]] == []
```
```

> Sources: 7 Sketches, Exercise 7.66 and Solution A.7.
