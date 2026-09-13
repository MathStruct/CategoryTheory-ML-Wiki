#solution #example

**Solution to [[7S Exercise 6.80|Exercise 6.80]].**

The decoration functor $\mathrm{Circ} : \mathbf{FinSet} \to \mathbf{Set}$ acts on a function $f$ by relabelling vertices: $\mathrm{Circ}(f)(V, A, s, t, \ell) = (V', A, f \mathbin{;} s, f \mathbin{;} t, \ell)$. So $\mathrm{Circ}(f)(c)$ has vertices $\{1, 2 \sim 3, 4\}$ and the $3\Omega$ resistor now runs from the merged vertex $2 \sim 3$ to $4$; the wire from $1$ ends at $2 \sim 3$.

````tabs
tab: Julia
```julia
using Catlab
# Circ(f) is the pushforward of the vertex set: Σ-migration along f on V
@present SchCircuit <: SchGraph begin Label::AttrType; label::Attr(E, Label) end
@acset_type Circuit(SchCircuit, index=[:src, :tgt])
c = @acset Circuit{String} begin V = 4; E = 1; src = [3]; tgt = [4]; label = ["3Ω"] end
f = [1, 2, 2, 3]                                    # 4 → 3, identifies 2 and 3
c′ = @acset Circuit{String} begin V = 3; E = 1; src = f[c[:src]]; tgt = f[c[:tgt]]; label = c[:label] end
```
````

> Sources: 7 Sketches, Exercise 6.80 and Solution A.6.
