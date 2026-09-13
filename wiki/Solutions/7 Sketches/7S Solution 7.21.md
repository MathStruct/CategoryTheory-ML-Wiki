#solution #example #program

**Solution to [[7S Exercise 7.21|Exercise 7.21]].**

1. $\mathsf{false}$. 2. $\mathsf{true}$. 3. $\mathsf{true}$. 4. The set is "even primes or $\geq 10$" $= \{2\} \cup \{10, 11, 12, \dots\}$; smallest three: $2, 10, 11$.

````tabs
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
````

> Sources: 7 Sketches, Exercise 7.21 and Solution A.7.
