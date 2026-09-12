#exercise #solution #example #program

**Exercise 6.86.** Express the two circuits of Eq. (6.73) as morphisms in $\mathbf{Cospan}_{\mathrm{Circ}}$ and compute their composite. Does it match Eq. (6.74)?

## Solution

The first is the cospan $\underline{1} \xrightarrow{f} V \xleftarrow{g} \underline{2}$, $f(1) = ul$, $g(1) = g(2) = ur$, decorated by the circuit $C$ of [[7S Exercise 6.79]]. The second is $\underline{2} \xrightarrow{f'} V' \xleftarrow{g'} \underline{2}$ with $V' = \{l, r, d\}$, $f'(1) = l$, $f'(2) = d$, $g'(1) = g'(2) = r$, decorated by $C' = (V', \{r_1', r_2'\}, s', t', \ell')$ with $r_1' : l \to r$ ($5\Omega$), $r_2' : r \to d$ ($8\Omega$).

Composing: the [[Pushout]] of $V \xleftarrow{g} \underline{2} \xrightarrow{f'} V'$ identifies $ur \sim l \sim d$ into one vertex $m$, giving $V'' = \{ul, dl, dr, m, r\}$ (five vertices), and the composite cospan $\underline{1} \to V'' \leftarrow \underline{2}$ has $1 \mapsto ul$ and $(1, 2) \mapsto (r, m)$. The decoration is $\mathrm{Circ}$ of the pushout applied to $\psi(C, C')$: arrows $r_1 : dl \to ul$, $r_2 : ul \to m$, $r_3 : m \to dr$, $c_1 : ul \to m$, $i_1 : dl \to dr$, $r_1' : m \to r$, $r_2' : r \to m$. This matches Eq. (6.74).

```tabs
tab: Julia
```julia
using Catlab
@present SchCircuit <: SchGraph begin Label::AttrType; label::Attr(E, Label) end
@acset_type Circuit(SchCircuit, index=[:src, :tgt])
const OpenCircuitOb, OpenCircuit = OpenACSetTypes(Circuit, :V)
# vertices 1=ul 2=ur 3=dl 4=dr
C = @acset Circuit{String} begin V = 4; E = 5
  src = [3, 1, 2, 1, 3]; tgt = [1, 2, 4, 2, 4]; label = ["1Ω", "2Ω", "1Ω", "3F", "1H"] end
C′ = @acset Circuit{String} begin V = 3; E = 2       # 1=l 2=r 3=d
  src = [1, 2]; tgt = [2, 3]; label = ["5Ω", "8Ω"] end
x = OpenCircuit{String}(C, FinFunction([1], 4), FinFunction([2, 2], 4))
y = OpenCircuit{String}(C′, FinFunction([1, 3], 3), FinFunction([2, 2], 3))
xy = compose(x, y)
nparts(apex(xy), :V), nparts(apex(xy), :E)          # (5, 7)
```
```

> Sources: 7 Sketches, Exercise 6.86 and Solution A.6.
