#definition #example #theorem

A **graph homomorphism** $\alpha : G \to H$ between [[Graph|graphs]] consists of functions $\alpha_V : G(V) \to H(V)$ and $\alpha_E : G(E) \to H(E)$ such that

$$
\alpha_V \circ G(\mathrm{src}) = H(\mathrm{src}) \circ \alpha_E \qquad\text{and}\qquad \alpha_V \circ G(\mathrm{tgt}) = H(\mathrm{tgt}) \circ \alpha_E,
$$

i.e. it "preserves sources and targets": an edge $a \to b$ goes to an edge $\alpha_V(a) \to \alpha_V(b)$. Exactly as a functor sends $A \to B$ to $F(A) \to F(B)$. The two conditions are commutative squares — the naturality squares of a [[Natural Transformation]] between $G, H : \mathsf{Gr} \to \mathbf{Set}$.

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
G(E) \arrow[r, "\alpha_E"] \arrow[d, "G(\mathrm{src})"'] & H(E) \arrow[d, "H(\mathrm{src})"] & G(E) \arrow[r, "\alpha_E"] \arrow[d, "G(\mathrm{tgt})"'] & H(E) \arrow[d, "H(\mathrm{tgt})"] \\
G(V) \arrow[r, "\alpha_V"'] & H(V) & G(V) \arrow[r, "\alpha_V"'] & H(V)
\end{tikzcd}
\end{document}
```

> Sources: Kittenlab Lecture 6 ("Sneak peak: natural transformations"), 7; 7 Sketches §3.3.5, Example 3.63, Exercise 3.64.

**Example 3.63/[[7S Exercise 3.64]].** $G = 1 \xrightarrow{a} 2 \xrightarrow{b} 3$, $H = 4 \xrightarrow{c, d} 5 \circlearrowleft e$. The unique homomorphism with $\alpha_E(a) = d$ has $\alpha_E(b) = e$, $\alpha_V(1) = 4$, $\alpha_V(2) = \alpha_V(3) = 5$.

**Example (Kittenlab).** A **three-colouring** of $G$ is a homomorphism into the triangle graph $K_3$: adjacent vertices get different colours because $K_3$ has no loops. Catlab's `homomorphisms` search solves such constraint problems.

[[Category of Graphs|$\mathbf{Grph}$]] is the category of graphs and graph homomorphisms; its isomorphisms are relabelings; monos are subgraph inclusions.

````tabs
tab: Julia
```julia
using Catlab
G = @acset Graph begin V = 3; E = 2; src = [1, 2]; tgt = [2, 3] end
H = @acset Graph begin V = 2; E = 3; src = [1, 1, 2]; tgt = [2, 2, 2] end
α = ACSetTransformation(G, H; V = [1, 2, 2], E = [2, 3])
is_natural(α)                                    # true
# three-colourings of the 5-cycle
K3 = complete_graph(Graph, 3)                    # (with both edge directions)
length(homomorphisms(cycle_graph(Graph, 5), K3)) # 30
```
tab: Lean
```lean
#check Prefunctor            -- V ⥤q W: obj and map, i.e. a graph homomorphism of quivers
#check SimpleGraph.Hom       -- homomorphisms of simple graphs
```
tab: Haskell
```haskell
-- check the two naturality squares on finite graphs
isGraphHom :: (Eq v') => Graph v e -> Graph v' e' -> (v -> v') -> (e -> e') -> Bool
isGraphHom g h fv fe = and [ fv (src g e) == src h (fe e) && fv (tgt g e) == tgt h (fe e) | e <- edges g ]
```
````
