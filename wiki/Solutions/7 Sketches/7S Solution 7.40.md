#solution #example #program

**Solution to [[7S Exercise 7.40|Exercise 7.40]].**

1. Six sections $(a_i, b_j, c_1)$ for $i \in \{1,2\}$, $j \in \{1,2,3\}$.
2. None: the fiber over $d$ is empty, so $\mathrm{Sec}_f(V_2) = \varnothing$.
3. Also none, for the same reason — $|\mathrm{Sec}_f(V_3)| = 2 \cdot 3 \cdot 0 \cdot 2 = 0$. (The printed solution says $12$, forgetting the empty fiber over $d$.)

````tabs
tab: Julia
```julia
fibers = Dict("a"=>2, "b"=>3, "c"=>1, "d"=>0, "e"=>2)
nsections(U) = prod(fibers[u] for u in U)
nsections(["a","b","c"]), nsections(["a","b","c","d"]), nsections(["a","b","d","e"])   # (6, 0, 0)
```
````

> Sources: 7 Sketches, Exercise 7.40 and Solution A.7.
