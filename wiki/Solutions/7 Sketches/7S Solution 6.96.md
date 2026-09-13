#solution #example #program

**Solution to [[7S Exercise 6.96|Exercise 6.96]].**

1. Two inner circles with two ports each, an outer circle with two ports, and three links: one port of the first circle is wired to a port of the second; the remaining port of each inner circle is wired to an outer port.
2. Three inner circles with two ports each, no outer ports; the links connect the circles in a chain.
3. $g \circ_1 f$ substitutes $f$ into the first circle of $g$; the operad composition is a pushout of the apices along the shared foot $\underline{2}$. Its arity is $(2, 2, 2, 2; 0)$: four inner circles and no outer ports.
4. The drawing shows $f$'s two circles sitting where the first circle of $g$ used to be — literally substitution of one wiring diagram into a circle of another.

````tabs
tab: Julia
```julia
using Catlab
f = UndirectedWiringDiagram(2)        # outer circle with 2 ports
add_box!(f, 2); add_box!(f, 2)        # two inner circles, 2 ports each
add_junctions!(f, 3)                  # apex 3
set_junction!(f, [1, 2, 2, 3])        # box ports → junctions
set_junction!(f, [1, 3], outer=true)  # outer ports → junctions
g = UndirectedWiringDiagram(0)
add_box!(g, 2); add_box!(g, 2); add_box!(g, 2)
add_junctions!(g, 3)
set_junction!(g, [1, 2, 2, 3, 3, 1])
h = ocompose(g, 1, f)                 # substitute f into box 1 of g
nboxes(h), length(ports(h, outer=true)), njunctions(h)   # (4, 0, 4)
```
````

> Sources: 7 Sketches, Exercise 6.96 and Solution A.6.
