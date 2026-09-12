#definition #example #program

An **undirected wiring diagram** (UWD) is a [[Wiring Diagram]] without input/output distinction: boxes with **ports**, an outer boundary with ports, and **junctions** to which ports are attached; a wire between two ports is a junction with two legs. A UWD with $n$ inner boxes of arities $a_1, \dots, a_n$ and outer arity $b$ is exactly an operation of the [[Operad]] $\mathbf{Cospan}$: a [[Cospan]] $a_1 + \cdots + a_n \to p \leftarrow b$ in $\mathbf{FinSet}$ whose apex $p$ is the set of junctions (7 Sketches Example 6.94). Kittenlab draws a cospan of finite sets in two styles: "cospan style" (elements and arrows into the apex) and "UWD style" (boxes, junction dots and wires).

> Sources: Kittenlab Lecture 15 (Fig. "Two styles of drawing an undirected wiring diagram", $\mathrm{Csp}(\mathcal{C})$); 7 Sketches §6.2.5 (Example 6.46, Eqs. 6.47, 6.50, Exercises 6.48–6.49), §6.3.2, §6.5 (Eqs. 6.89–6.90, 6.95); Catlab `UndirectedWiringDiagram`, `@relation`, `oapply`.

- Composition of cospans by [[Pushout]] is, in wire terms, "the composite has one apex element per connected component of the concatenated wire diagrams, and each foot element is wired to its component" ([[7S Exercise 6.49]]); the monoidal product stacks diagrams ([[7S Exercise 6.48]]).
- UWDs are the string diagrams of [[Hypergraph Category|hypergraph categories]]: junctions are spiders ([[Frobenius Monoid]]), and the Frobenius equations say only connectivity matters (Theorem 6.55).
- In Catlab a UWD is an [[C-Set|ACSet]] on the schema with objects Box, Port, OuterPort, Junction and morphisms $\mathrm{box} : \mathrm{Port} \to \mathrm{Box}$, $\mathrm{junction} : \mathrm{Port} \to \mathrm{Junction}$, $\mathrm{outer\_junction} : \mathrm{OuterPort} \to \mathrm{Junction}$; `@relation` writes one as a conjunctive query (a relational join), and `oapply` evaluates an [[Operad Algebra]] on it — hence UWDs are also the syntax of database *queries*, relational composition and [[Data Migration Functor|$\Pi$-migrations]].
- [[Open Graph|Open graphs]], open [[Petri Net|Petri nets]] and open circuits ([[Decorated Cospan]]) are cospans whose apex carries extra structure; their composition follows the UWD pattern.

````tabs
tab: Julia
```julia
using Catlab, Catlab.WiringDiagrams, Catlab.Programs
uwd = @relation (x, z) begin
  R(x, y); S(y, z)
end
uwd                                      # an ACSet: Box, Port, OuterPort, Junction tables
nboxes(uwd), njunctions(uwd)             # (2, 3)
# the corresponding cospan 2 + 2 → 3 ← 2: ports ↦ junctions and outer ports ↦ junctions
uwd[:junction], uwd[:outer_junction]     # ([1, 3, 3, 2], [1, 2])
# Graphics: to_graphviz(uwd) draws boxes, junctions and wires
```
tab: Haskell
```haskell
-- a UWD as a cospan of finite sets: inner ports and outer ports mapped to junctions
data UWD = UWD { boxArities :: [Int], portJunction :: [Int], outerJunction :: [Int], nJunctions :: Int }
```
````
