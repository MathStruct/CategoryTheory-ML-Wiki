#solution #program

**Solution to [[DaoFP Exercise 7.1.2|Exercise 7.1.2]].**

```haskell
plusC :: Nat -> (Nat -> Nat)
plusC = rec init step
  where
    init = id                  -- 0 + m = m
    step f = S . f             -- (n+1) + m = S (n + m)
```

Here the target of the recursor is the exponential $N^N$; `init` is the element $\mathrm{id} \in N^N$ and `step` post-composes with $S$.

> Sources: DaoFP Exercise 7.1.2.
