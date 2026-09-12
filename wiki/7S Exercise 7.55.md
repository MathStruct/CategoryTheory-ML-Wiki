#exercise #solution #example #program

**Exercise 7.55.** Let $G$ be the graph $A \xrightarrow{f} B$, $A \xrightarrow{g} B$, $B \xrightarrow{h} C$, $C \xrightarrow{i} D$ and $H \subseteq G$ the subgraph with vertices $A, B, C$ and the single arrow $f$. Find the classifying graph homomorphism $\ulcorner H \urcorner : G \to \Omega_{\mathbf{Grph}}$ ([[Topos of Graphs]]).

## Solution

Write $\gamma = \ulcorner H \urcorner$. Vertices: $\gamma(A) = \gamma(B) = \gamma(C) = V$ (present), $\gamma(D) = 0$ (missing). Arrows: $\gamma(f) = (V, V; A)$ (present); $\gamma(g) = \gamma(h) = (V, V; 0)$ (endpoints present, arrow missing); $\gamma(i) = (V, 0; 0)$ (source present, target missing). This is the unique homomorphism whose pullback of $\mathsf{true}$ is $H$.

```tabs
tab: Julia
```julia
using Catlab
Ω, _ = subobject_classifier(Graph)     # vertex 1 = V, 2 = 0; edges 1=(V,V;A) 2=(V,V;0) 3=(V,0;0) 4=(0,V;0) 5=(0,0;0)
G = @acset Graph begin V = 4; E = 4; src = [1, 1, 2, 3]; tgt = [2, 2, 3, 4] end   # A,B,C,D; f,g,h,i
γ = ACSetTransformation(G, Ω; V=[1, 1, 1, 2], E=[1, 2, 2, 3])
is_natural(γ)                           # true
```
```

> Sources: 7 Sketches, Exercise 7.55 and Solution A.7.
