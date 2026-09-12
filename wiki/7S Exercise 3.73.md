#exercise #solution

**Exercise 3.73.** For the [[Currying|currying]] adjunction $(- \times B) \dashv (-)^B$: 1. what does $- \times B$ do to $f : X \to Y$? 2. what does $(-)^B$ do? 3. currying $+ : \mathbb{N} \times \mathbb{N} \to \mathbb{N}$ gives $p : \mathbb{N} \to \mathbb{N}^{\mathbb{N}}$; what is $p(3)$?

## Solution

1. $f \times B : (x, b) \mapsto (f(x), b)$ — functorial. 2. $f^B : g \mapsto g \mathbin{;} f$ for $g : B \to X$ — functorial since $(f_1 \mathbin{;} f_2)^B = f_1^B \mathbin{;} f_2^B$. 3. $p(3) : \mathbb{N} \to \mathbb{N}$ is $n \mapsto n + 3$, "the function that adds three".
