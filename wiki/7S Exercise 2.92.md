#exercise #solution

**Exercise 2.92.** 1. What is $\bigvee \varnothing$ ("$0$") in $\mathbf{Bool}$ and in $\mathbf{Cost}$? 2. What is $x \vee y$ in each?

## Solution

1a. $\mathsf{false}$, the least element. 1b. $\infty$: because [[Cost]] uses the reversed order $\geq$, $\infty$ is the least element — so the "$0$" of Definition 2.90 is $\infty$ here; beware.
2a. OR. 2b. $\min(x, y)$, the greatest number $\leq$ both under the usual order.
