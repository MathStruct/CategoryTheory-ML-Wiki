#exercise #solution #proof

**Exercise 1.94.** Prove that for any [[Monotone Map]] $f : P \to Q$, if $a \vee b$ and $f(a) \vee f(b)$ exist then $f(a) \vee f(b) \leq f(a \vee b)$.

> Context: [[Generative Effect]] — the effect always produces *more*, never merely *different*.

## Solution

Since $a \leq a \vee b$ and $b \leq a \vee b$, monotonicity gives $f(a) \leq f(a \vee b)$ and $f(b) \leq f(a \vee b)$. So $f(a \vee b)$ is an upper bound of $\{f(a), f(b)\}$, and the join $f(a) \vee f(b)$ is the least one: $f(a) \vee f(b) \leq f(a \vee b)$.
