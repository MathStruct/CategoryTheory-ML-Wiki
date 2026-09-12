#exercise #solution #example #program

**Exercise 6.79.** Write a tuple $(V, A, s, t, \ell)$ representing the circuit of Eq. (6.71) (a square with resistors on three sides, a capacitor on the top, and an inductor on the diagonal).

## Solution

$V = \{ul, ur, dl, dr\}$, $A = \{r_1, r_2, r_3, c_1, i_1\}$, with

| | $r_1$ | $r_2$ | $r_3$ | $c_1$ | $i_1$ |
|---|---|---|---|---|---|
| $s$ | dl | ul | ur | ul | dl |
| $t$ | ul | ur | dr | ur | dr |
| $\ell$ | $1\Omega$ | $2\Omega$ | $1\Omega$ | $3F$ | $1H$ |

A $\mathcal{C}$-circuit is precisely an edge-labelled [[Graph]] ([[C-Set]] on the schema of graphs with a label attribute).

```tabs
tab: Julia
```julia
using Catlab
@present SchCircuit <: SchGraph begin
  Label::AttrType
  label::Attr(E, Label)
end
@acset_type Circuit(SchCircuit, index=[:src, :tgt])
# vertices 1=ul 2=ur 3=dl 4=dr
c = @acset Circuit{String} begin
  V = 4; E = 5
  src = [3, 1, 2, 1, 3]; tgt = [1, 2, 4, 2, 4]
  label = ["1Ω", "2Ω", "1Ω", "3F", "1H"]
end
```
tab: Haskell
```haskell
data Circuit v l = Circuit { arrows :: [(v, v, l)] }   -- (s a, t a, ℓ a)
c :: Circuit String String
c = Circuit [("dl","ul","1Ω"),("ul","ur","2Ω"),("ur","dr","1Ω"),("ul","ur","3F"),("dl","dr","1H")]
```
```

> Sources: 7 Sketches, Exercise 6.79 and Solution A.6.
