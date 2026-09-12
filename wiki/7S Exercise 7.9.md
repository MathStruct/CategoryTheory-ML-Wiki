#exercise #solution #example #program

**Exercise 7.9.** Factor the function $f : \underline{3} \to \underline{3}$ (with $f(1) = f(2) = 1$, $f(3) = 2$ in the picture) as an [[Epimorphism]] followed by a [[Monomorphism]] ([[Epi-Mono Factorization]]).

## Solution

$\underline{3} \twoheadrightarrow \underline{2} \hookrightarrow \underline{3}$: first the surjection $1, 2 \mapsto 1$, $3 \mapsto 2$ onto the image $\{1, 2\}$, then the inclusion of the image into $\underline{3}$.

```tabs
tab: Julia
```julia
using Catlab
f = FinFunction([1, 1, 2], 3)
e, m = epi_mono(f)
collect(e), collect(m)          # ([1, 1, 2], [1, 2])
compose(e, m) == f              # true
```
```

> Sources: 7 Sketches, Exercise 7.9 and Solution A.7.
