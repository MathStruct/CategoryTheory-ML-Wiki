#exercise #solution #proof

**Exercise 6.63.** Show, using colimits, that the Frobenius maps on $\mathbf{Cospan}_{\mathbf{FinSet}}$ (Example 6.61) obey the special law $\delta \mathbin{;} \mu = \mathrm{id}$ ([[Frobenius Monoid]]).

## Solution

The composite $\delta \mathbin{;} \mu$ is the cospan $X \xrightarrow{\mathrm{id}} X \xleftarrow{[\mathrm{id},\mathrm{id}]} X + X \xrightarrow{[\mathrm{id},\mathrm{id}]} X \xleftarrow{\mathrm{id}} X$, so it suffices to show that the square with $X + X$ at the top left, two copies of $[\mathrm{id}, \mathrm{id}] : X + X \to X$, and two identities $X \to X$ is a [[Pushout]]. It commutes trivially. Given $f, g : X \to T$ with $[\mathrm{id},\mathrm{id}] \mathbin{;} f = [\mathrm{id},\mathrm{id}] \mathbin{;} g$, precomposing with $\iota_1 : X \to X + X$ and using $\iota_1 \mathbin{;} [\mathrm{id},\mathrm{id}] = \mathrm{id}$ ([[7S Exercise 6.17]]) gives $f = g$. Then $f$ is the unique map from the apex $X$ making the cocone commute. Hence the pushout apex is $X$ with identity legs, i.e. $\delta \mathbin{;} \mu = \mathrm{id}_X$.

```tabs
tab: Julia
```julia
using Catlab
X = FinSet(2)
δ = Cospan(id(X), FinFunction([1, 2, 1, 2], 2))
μ = Cospan(FinFunction([1, 2, 1, 2], 2), id(X))
P = pushout(right(δ), left(μ))
ob(P)                                # FinSet(2): the special law holds
```
```

> Sources: 7 Sketches, Exercise 6.63 and Solution A.6.
