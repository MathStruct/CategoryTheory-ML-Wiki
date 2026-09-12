#exercise #solution #proof

**Exercise 1.109.** Complete the proof of Proposition 1.107: 1. if $f \dashv g$ then $f(g(q)) \leq q$; 2. if $p \leq g(f(p))$ and $f(g(q)) \leq q$ for all $p, q$, then $p \leq g(q)$ iff $f(p) \leq q$.

> See [[Galois Connection]].

## Solution

1. Apply the definition with $p := g(q)$ to the reflexivity fact $g(q) \leq g(q)$: $f(g(q)) \leq q$.
2. If $p \leq g(q)$, apply $f$: $f(p) \leq f(g(q)) \leq q$. If $f(p) \leq q$, apply $g$: $p \leq g(f(p)) \leq g(q)$.
