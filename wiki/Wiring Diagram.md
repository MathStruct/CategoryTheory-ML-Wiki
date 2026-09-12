#definition #annotation

**Wiring diagrams** (string diagrams) are visual representations for building new relationships from old. Boxes are relationships, wires are objects/resources, and the diagram itself shows how relationships combine. Invented in the context of [[Monoidal Category|monoidal categories]] by Joyal and Street, they have long been used informally by engineers and scientists. Wires and boxes are **icons**; more icons appear as more structure is assumed.

> Sources: 7 Sketches §2.2.2 (Eq. 2.13 "different styles"), §4.4.2, §5.2, §6.3.2, §6.5.1; DaoFP §15.1 (string diagrams for monads and adjunctions); Kittenlab Lecture 6 (directed port graphs, wiring diagrams as ACSets), 15 (undirected wiring diagrams as cospans).

## Styles, by structure

| structure | diagram style | note |
|---|---|---|
| [[Preorder]] | single-input, single-output boxes in series | chaining $x_0 \leq x_1 \leq x_2$ |
| [[Symmetric Monoidal Preorder]] | boxes with many inputs/outputs in series **and parallel**; crossing wires | [[Wiring Diagrams for Monoidal Preorders]] |
| + [[Discard and Copy Axioms|discard axiom]] | wires may terminate | manufacturing |
| + copy axiom | wires may split | informatics |
| [[Category]] | boxes with one input and one output in series | morphisms; the [[Free Category]] on a graph |
| [[Monoidal Category]] | as for monoidal preorders, but boxes are *named* morphisms (§4.4.2) | [[Symmetric Monoidal Category]] diagrams |
| [[Prop]] | boxes with $m$ inputs and $n$ outputs on the objects $\underline{n}$ | [[Port Graph|port graphs]], [[Signal Flow Graph|signal flow graphs]] |
| [[Compact Closed Category]] | wires may bend backwards (cups and caps) | [[Feasibility Relation|Feas]] |
| [[Hypergraph Category]] | wires may split, merge, start and end freely (spiders) | [[Undirected Wiring Diagram|undirected wiring diagrams]], [[Cospan|cospans]] |
| [[Operad]] | boxes nested inside boxes, composed by substitution | $\mathbf{Cospan}$-algebras, [[Decorated Cospan|decorated cospans]] |
| 2-categories / [[Adjunction|adjunctions]] | string diagrams with regions for categories, strings for functors, dots for natural transformations | DaoFP §15.1 |

**Soundness.** A wiring diagram is a *graphical proof*: if all interior boxes are valid, the exterior box is valid ([[Wiring Diagrams for Monoidal Preorders]]). In a monoidal category two diagrams that are isotopic denote equal morphisms — the coherence theorem of Joyal–Street; for [[Operad|operads]] a wiring diagram is literally a morphism of the operad $\mathbf{Cospan}$ or $\mathbf{Set}$.

## Wiring diagrams as data (Kittenlab Lecture 6)

A **directed port graph** is an [[C-Set|ACSet]] on the schema with boxes, input ports, output ports and wires (source: an output port, target: an input port); a directed wiring diagram additionally has outer ports. Catlab's `WiringDiagram` and `UndirectedWiringDiagram` implement these, and `oapply` evaluates an [[Operad|operad algebra]] on a diagram.

````tabs
tab: Julia
```julia
using Catlab, Catlab.WiringDiagrams
# a directed wiring diagram with two boxes composed in series (Catlab.WiringDiagrams)
f = Box(:f, [:A], [:B]); g = Box(:g, [:B], [:C])
d = WiringDiagram([:A], [:C])
fv, gv = add_box!(d, f), add_box!(d, g)
add_wires!(d, [(input_id(d), 1) => (fv, 1), (fv, 1) => (gv, 1), (gv, 1) => (output_id(d), 1)])
nboxes(d), nwires(d)      # (2, 3)

# an undirected wiring diagram via the @relation macro (hypergraph style)
uwd = @relation (x, z) begin
  R(x, y); S(y, z)
end
```
tab: Haskell
```haskell
-- a wiring diagram as a free symmetric monoidal expression (syntax tree)
data WD = Box String [String] [String]   -- name, input wires, output wires
        | Seq WD WD                      -- series composition
        | Par WD WD                      -- parallel composition
        | Id [String]
        | Swap String String
```
````
