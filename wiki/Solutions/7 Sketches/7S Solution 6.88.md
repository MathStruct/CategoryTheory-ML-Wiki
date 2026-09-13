#solution #example

**Solution to [[7S Exercise 6.88|Exercise 6.88]].**

$\eta \mathbin{;} x$ identifies the two left terminals of $x$ into one vertex (a wire closing the left side); $\eta \mathbin{;} x \mathbin{;} \epsilon$ then also identifies the two right terminals. The result is a cospan $\underline{0} \to N \leftarrow \underline{0}$ decorated with the circuit of $x$ in which the left pair and the right pair of terminals are each merged: a closed loop with no exposed terminals. Such closed circuits are the scalars $I \to I$ of the [[Hypergraph Category]].

````tabs
tab: Julia
```julia
using Catlab
@present SchCircuit <: SchGraph begin Label::AttrType; label::Attr(E, Label) end
@acset_type Circuit(SchCircuit, index=[:src, :tgt])
const OpenCircuitOb, OpenCircuit = OpenACSetTypes(Circuit, :V)
x = @acset Circuit{Symbol} begin V = 4; E = 2; src = [1, 3]; tgt = [2, 4]; label = [:battery, :resistor] end
ox = OpenCircuit{Symbol}(x, FinFunction([1, 3], 4), FinFunction([2, 4], 4))
empty1 = @acset Circuit{Symbol} begin V = 1 end
η = OpenCircuit{Symbol}(empty1, FinFunction(Int[], 1), FinFunction([1, 1], 1))
ε = OpenCircuit{Symbol}(empty1, FinFunction([1, 1], 1), FinFunction(Int[], 1))
closed = compose(η, ox, ε)
nparts(apex(closed), :V), nparts(apex(closed), :E)   # (2, 2): a closed loop
```
````

> Sources: 7 Sketches, Exercise 6.88 and Solution A.6.
