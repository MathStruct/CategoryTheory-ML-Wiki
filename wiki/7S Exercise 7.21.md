#exercise #solution #example #program

**Exercise 7.21.** Let $E$ = evens, $P$ = primes, $T = \{n \geq 10\}$, subsets of $\mathbb{N}$ with characteristic functions $\ulcorner E \urcorner, \ulcorner P \urcorner, \ulcorner T \urcorner : \mathbb{N} \to \mathbb{B}$. 1. $\ulcorner E \urcorner(17)$? 2. $\ulcorner P \urcorner(17)$? 3. $\ulcorner T \urcorner(17)$? 4. The smallest three elements of the set classified by $(\ulcorner E \urcorner \wedge \ulcorner P \urcorner) \vee \ulcorner T \urcorner$?

## Solution

1. $\mathsf{false}$. 2. $\mathsf{true}$. 3. $\mathsf{true}$. 4. The set is "even primes or $\geq 10$" $= \{2\} \cup \{10, 11, 12, \dots\}$; smallest three: $2, 10, 11$.

```tabs
tab: Julia
```julia
using Primes
E(n) = iseven(n); P(n) = isprime(n); T(n) = n >= 10
[n for n in 0:20 if (E(n) && P(n)) || T(n)][1:3]      # [2, 10, 11]
```
tab: Haskell
```haskell
isPrime n = n > 1 && all (\d -> n `mod` d /= 0) [2 .. n - 1]
classified = [ n | n <- [0 ..], (even n && isPrime n) || n >= 10 ]   -- take 3 → [2,10,11]
```
```

> Sources: 7 Sketches, Exercise 7.21 and Solution A.7.
