#exercise #solution #proof

**Exercise 1.80.** 1. Why is $0$ a lower bound for $S = \{\frac{1}{n+1} \mid n \in \mathbb{N}\} \subseteq \mathbb{R}$? 2. Why is it the greatest lower bound ([[Meet]])?

## Solution

1. $0 \leq \frac{1}{n+1}$ for all $n$.
2. Suppose $b$ is a lower bound with $0 < b$. Pick $n$ with $1/b < n + 1$; then $\frac{1}{n+1} < b$, contradicting that $b$ is a lower bound. So every lower bound is $\leq 0$.
