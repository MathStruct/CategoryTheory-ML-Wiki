#exercise #solution #proof

**Exercise 6.28.** In Example 6.27 ([[Pushout]] over an [[Initial Object]] is a [[Coproduct]]) justify the three "why?"s.

## Solution

1. The square $X \leftarrow \varnothing \to Y$, $X \to X + Y \leftarrow Y$ commutes because there is only one map $\varnothing \to X + Y$; so $f \mathbin{;} \iota_X = g \mathbin{;} \iota_Y$.
2. Given $x : X \to T$, $y : Y \to T$ (the square with $\varnothing$ commutes automatically), the universal property of the coproduct gives a unique $[x, y] : X + Y \to T$ with $\iota_X \mathbin{;} [x, y] = x$ and $\iota_Y \mathbin{;} [x, y] = y$.
3. Conversely, if the pushout $X +_\varnothing Y$ exists then for any $x, y$ as above, the outer square commutes (again because $\varnothing$ is initial), so the pushout property gives a unique $t : X +_\varnothing Y \to T$ with $\iota_X \mathbin{;} t = x$, $\iota_Y \mathbin{;} t = y$. This is exactly the universal property of the coproduct.

> Sources: 7 Sketches, Exercise 6.28 and Solution A.6.
