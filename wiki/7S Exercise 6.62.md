#exercise #solution #example

**Exercise 6.62.** $\mathbf{Cospan}_{\mathbf{FinSet}}$ is a [[Hypergraph Category]]. Draw the Frobenius morphisms for the object $\underline{1}$ using both the function and wiring depictions.

## Solution

| | cospan | wiring |
|---|---|---|
| multiplication $\mu$ | $\underline{2} \xrightarrow{[\mathrm{id},\mathrm{id}]} \underline{1} \xleftarrow{\mathrm{id}} \underline{1}$ | two wires merging into one |
| unit $\eta$ | $\underline{0} \xrightarrow{!} \underline{1} \xleftarrow{\mathrm{id}} \underline{1}$ | a wire starting from nothing |
| comultiplication $\delta$ | $\underline{1} \xrightarrow{\mathrm{id}} \underline{1} \xleftarrow{[\mathrm{id},\mathrm{id}]} \underline{2}$ | one wire splitting into two |
| counit $\epsilon$ | $\underline{1} \xrightarrow{\mathrm{id}} \underline{1} \xleftarrow{!} \underline{0}$ | a wire ending in nothing |

The empty set is depicted as blank space.

```tabs
tab: Julia
```julia
using Catlab
μ = Cospan(FinFunction([1, 1], 1), FinFunction([1], 1))
η = Cospan(FinFunction(Int[], 1), FinFunction([1], 1))
δ = Cospan(FinFunction([1], 1), FinFunction([1, 1], 1))
ε = Cospan(FinFunction([1], 1), FinFunction(Int[], 1))
```
```

> Sources: 7 Sketches, Exercise 6.62 and Solution A.6.
