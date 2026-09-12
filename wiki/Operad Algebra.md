#definition #theorem #example

An **algebra** for an [[Operad]] $\mathcal{O}$ is an operad functor $F : \mathcal{O} \to \mathbf{Set}$: it assigns to each type $t$ a set $F(t)$ of **fillers** for boxes of type $t$, and to each operation (wiring diagram) $f \in \mathcal{O}(t_1, \dots, t_n; t)$ a function $F(f) : F(t_1) \times \cdots \times F(t_n) \to F(t)$ that assembles $n$ fillers into one — e.g. "takes $n$ circuits with interfaces $t_1, \dots, t_n$ and returns a circuit with boundary $t$". Just as set-valued functors $\mathcal{C} \to \mathbf{Set}$ are [[C-Set|database instances]], set-valued operad functors are the applications obeying a compositional grammar.

> Sources: 7 Sketches §6.5.3 (Definition 6.99, Example 6.100, Proposition 6.101), Remark 5.74 (algebraic theories), §6.6; Catlab `oapply`.

**Example 6.100 (circuits).** $\mathbf{Circ} : \mathbf{Cospan} \to \mathbf{Set}$ sends $t \in \mathbb{N}$ to the set of circuits with $t$ marked terminals; a wiring diagram $\varphi \in \mathbf{Cospan}(2, 2, 2; 0)$ gives $\mathbf{Circ}(\varphi) : \mathbf{Circ}(2)^3 \to \mathbf{Circ}(0)$, which applied to a battery, a switch and a lamp-with-resistor yields the closed light-switch circuit of §6.1. This is the [[Decorated Cospan|decorated-cospan]] story in operadic form.

**Proposition 6.101.** $\mathbf{Cospan}$-algebras are equivalent to [[Hypergraph Category|hypergraph props]] — so $\mathbf{Cospan}$ is "the theory of hypergraph categories" (cf. [[Frobenius Monoid|Theorem 6.58]]). Advantages of the operadic view: every hypergraph prop arises (decorated cospans give only some), and one may vary the operad ($\mathbf{Cospan}$ → cobordisms → wiring diagrams without passing wires) to get different compositionality rules. Compare: [[Monoid Object|monoid objects]] are algebras of the prop "theory of monoids" (Remark 5.74); Catlab's `oapply(diagram, fillers)` evaluates an operad algebra — for finite relations, [[Petri Net|Petri nets]], dynamical systems, or circuits — on an [[Undirected Wiring Diagram]].

````tabs
tab: Julia
```julia
using Catlab, Catlab.WiringDiagrams, Catlab.Programs
# the operad algebra of finite relations on UWDs: fillers are relations (as tables), oapply composes them
uwd = @relation (x, z) begin
  R(x, y); S(y, z)
end
# fillers: relations R ⊆ {1,2}×{1,2}, S ⊆ {1,2}×{1,2} as ACSets / tables
R = FinRelation((a, b) -> a <= b, 2, 2)   # schematic; Catlab's `oapply` works with e.g. Petri nets in AlgebraicPetri
# In AlgebraicPetri: oapply(uwd, Dict(:R => open_petri_1, :S => open_petri_2)) glues open Petri nets.
```
tab: Haskell
```haskell
-- an algebra assigns filler sets to types and an assembly function to each wiring diagram (schematic)
class Operad op => Algebra op a where
  assemble :: op -> [a] -> a       -- F(f)(fillers)
```
````
