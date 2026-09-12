#exercise #solution

**Exercise 1.103.** With $S = \{1,2,3,4\}$, $T = \{12, 3, 4\}$ and $g$ as in Example 1.102, choose six partitions $c$ of $S$ and compute the pushforward $g_!(c)$.

> See [[Pushforward and Pullback of Partitions]].

## Solution (four of them)

$g_!((1)(2)(3)(4)) = (12)(3)(4)$; $g_!((12)(3)(4)) = (12)(3)(4)$; $g_!((13)(2)(4)) = (12\,3)(4)$; $g_!((1)(2)(34)) = (12)(34)$; $g_!((14)(23)) = (12\,3\,4)$; $g_!((1)(234)) = (12\,3\,4)$. In general merge $1, 2$ into $12$ and take the transitive closure.
