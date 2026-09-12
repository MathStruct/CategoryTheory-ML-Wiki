#exercise #solution #example

**Exercise 7.49.** For the [[Sierpinski Space]]: 1. What is the category $\mathrm{Op}$? 2. What is a [[Presheaf]] on it? 3. What is the [[Sheaf|sheaf condition]]? 4. How do we identify a sheaf with a function?

## Solution

1. The chain $\varnothing \to \{1\} \to \{1,2\}$.
2. Three sets and two functions $F(\{1,2\}) \to F(\{1\}) \to F(\varnothing)$.
3. The only non-trivial cover is the empty cover of $\varnothing$ ([[7S Exercise 7.31]]), whose sheaf condition is $F(\varnothing) = \{()\}$ (Example 7.36).
4. Hence a sheaf is a set $F(\{1,2\})$, a set $F(\{1\})$ and a function between them; $\mathbf{Shv}(\text{Sierpiński})$ is equivalent to the arrow category $\mathbf{Set}^{\to}$.

> Sources: 7 Sketches, Exercise 7.49 and Solution A.7.
