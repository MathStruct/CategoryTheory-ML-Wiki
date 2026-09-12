#exercise #solution

**Exercise 2.44.** Consider $d, u : [0, \infty] \to \mathbb{B}$ with $d(x) = [x = 0]$ and $u(x) = [x < \infty]$. Are they monotone, monoidal monotone, strict?

## Solution

Yes to everything: both are strict [[Monoidal Monotone Map|monoidal monotones]] $\mathbf{Cost} \to \mathbf{Bool}$. $d$ asks "is $x = 0$?": $0$ is $0$, and a sum is $0$ iff both summands are. $u$ asks "is $x$ finite?": $0$ is finite, and a sum is finite iff both summands are. They give two different [[Change of Base|changes of base]] from [[Lawvere Metric Space|metric spaces]] to preorders ([[7S Exercise 2.68]]).
