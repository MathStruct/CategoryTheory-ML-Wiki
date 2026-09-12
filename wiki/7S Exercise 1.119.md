#exercise #solution #proof

**Exercise 1.119.** Suppose $f \dashv g$. Show 1. $p \leq (f \mathbin{;} g)(p)$; 2. $(f \mathbin{;} g \mathbin{;} f \mathbin{;} g)(p) \cong (f \mathbin{;} g)(p)$.

> Hence $f \mathbin{;} g$ is a [[Closure Operator]].

## Solution

1. This is the unit inequality of Proposition 1.107.
2. $\geq$: apply (1) to $g(f(p))$. $\leq$: the counit gives $f(g(f(p))) \leq f(p)$; apply the monotone $g$ to get $g(f(g(f(p)))) \leq g(f(p))$.
