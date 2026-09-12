#exercise #solution #example

**Exercise 7.72.** Let $S$ be the sheaf of people and $j$ = "assuming Bob is in San Diego, ..." ([[Modality]] of type (a)). 1. Name a predicate $p$. 2. For a time interval $U$ and $s \in S(U)$, what is $p(s)$? 3. What is $j(p(s))$? 4. Is $p(s) \leq j(p(s))$? 5. Is $j(j(p(s))) = j(p(s))$? 6. For another $q$, is $j(p \wedge q) = j(p) \wedge j(q)$?

## Solution

1. $p(s)$ = "$s$ likes the weather".
2. $U$ = January 2019; $p(s) \subseteq U$ is the sub-interval throughout which $s$ likes the weather.
3. $j(p(s)) = (\text{Bob in SD}) \Rightarrow p(s)$: the times at which either Bob is not in San Diego or $s$ likes the weather.
4. Yes, by 3.
5. Yes: "if Bob is in SD then (if Bob is in SD then $p$)" is equivalent to "if Bob is in SD then $p$".
6. Yes, with $q$ = "$s$ is happy": "if Bob is in SD then ($p$ and $q$)" iff ("if Bob is in SD then $p$" and "if Bob is in SD then $q$").

> Sources: 7 Sketches, Exercise 7.72 and Solution A.7.
