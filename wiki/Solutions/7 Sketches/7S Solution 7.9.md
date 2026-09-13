#solution #example #program

**Solution to [[7S Exercise 7.9|Exercise 7.9]].**

$\underline{3} \twoheadrightarrow \underline{2} \hookrightarrow \underline{3}$: first the surjection $1, 2 \mapsto 1$, $3 \mapsto 2$ onto the image $\{1, 2\}$, then the inclusion of the image into $\underline{3}$.

````tabs
tab: Julia
```julia
using Catlab
f = FinFunction([1, 1, 2], 3)
e, m = epi_mono(f)
collect(e), collect(m)          # ([1, 1, 2], [1, 2])
compose(e, m) == f              # true
```
````

> Sources: 7 Sketches, Exercise 7.9 and Solution A.7.
