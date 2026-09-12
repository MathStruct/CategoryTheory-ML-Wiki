#exercise #solution #example

**Exercise 6.6.** For each of the graphs below, consider the [[Free Category]] on that graph and say whether it has an [[Initial Object]].
1. a single vertex $a$; 2. $a \to b \to c$; 3. two vertices $a$, $b$ and no edges; 4. a vertex $a$ with a loop.

## Solution

The objects of a free category are the vertices and the morphisms are paths, so an initial object is a vertex with exactly one path to every vertex (including itself).
1. Yes: $a$, whose only path to itself is the empty path.
2. Yes: $a$ has a unique path to $a$, $b$, $c$.
3. No: there is no path from $a$ to $b$ nor from $b$ to $a$.
4. No: $a$ has infinitely many paths to itself (the loop iterated $n$ times), so the hom-set $\mathcal{C}(a, a)$ is not a singleton.

> Sources: 7 Sketches, Exercise 6.6 and Solution A.6.
