#solution #example

**Solution to [[7S Exercise 6.82|Exercise 6.82]].**

$\psi_{V,V'}$ takes the disjoint union of labelled graphs: $\psi_{2,2}(b, s)$ is the 4-vertex circuit consisting of the battery between vertices $1, 2$ and the switch between vertices $3, 4$, with no connection between them. This laxator is what makes $\mathrm{Circ}$ a lax [[Monoidal Functor]], and thus $\mathbf{Cospan}_{\mathrm{Circ}}$ a [[Hypergraph Category]] ([[Decorated Cospan]]).

````tabs
tab: Julia
```julia
using Catlab
@present SchCircuit <: SchGraph begin Label::AttrType; label::Attr(E, Label) end
@acset_type Circuit(SchCircuit, index=[:src, :tgt])
b = @acset Circuit{Symbol} begin V = 2; E = 1; src = [1]; tgt = [2]; label = [:battery] end
s = @acset Circuit{Symbol} begin V = 2; E = 1; src = [1]; tgt = [2]; label = [:switch] end
ψ = ob(coproduct(b, s))          # ψ₂,₂(b, s): 4 vertices, 2 edges
```
````

> Sources: 7 Sketches, Exercise 6.82 and Solution A.6.
