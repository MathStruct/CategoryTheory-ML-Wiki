#example #theorem

The [[Category of Graphs|category $\mathbf{Grph}$]] of directed graphs is a presheaf [[Topos]]: graphs are [[Presheaf|presheaves]] on the arrow shape $\mathbf{ArShp} = (\mathrm{Vertex} \rightrightarrows \mathrm{PureArrow})$, i.e. instances on the schema $\mathbf{Gr} = \mathbf{ArShp}^{\mathrm{op}}$ (Example 7.23). Its [[Subobject Classifier]] $\Omega_{\mathbf{Grph}}$ is itself a graph with two vertices $0, V$ and five arrows:

$$
(0,0;0) : 0 \to 0, \quad (0,V;0) : 0 \to V, \quad (V,0;0) : V \to 0, \quad (V,V;0) : V \to V, \quad (V,V;A) : V \to V.
$$

The [[Terminal Object|terminal graph]] is one vertex with one loop, and $\mathsf{true} : 1 \to \Omega$ sends that loop to $(V, V; A)$.

> Sources: 7 Sketches Example 7.23 ("presheaves on $\mathbf{ArShp}$ are just directed graphs", "a graph is a sort of lego construction"), Example 7.54, Exercise 7.55, [Vig03]; Kittenlab Lecture 6 (graphs as C-sets).

**Classifying a subgraph.** Given $H \subseteq G$, the characteristic map $\ulcorner H \urcorner : G \to \Omega$ records "how much of each part is in $H$": a vertex goes to $V$ if it is in $H$ and to $0$ otherwise; an arrow in $H$ goes to $(V,V;A)$; an arrow not in $H$ goes to $(V,0;0)$, $(0,V;0)$, $(V,V;0)$ or $(0,0;0)$ according to whether its source and target are in $H$. See [[7S Chapter 7 Exercises#Exercise 7.55|7S Exercise 7.55]]. The name of each arrow of $\Omega$ lists which of (source, target; arrow) are present — this is what the [[Yoneda Lemma]] produces: $\Omega(c)$ is the set of subobjects of the [[Representable Functor|representable]] $y(c)$.

The lattice $\mathrm{Sub}(G)$ is a [[Heyting Algebra]] but not Boolean: the negation of a subgraph $A$ is the largest subgraph disjoint from $A$, so an edge touching $A$ is in neither $A$ nor $\neg A$ ([[Internal Logic of a Topos]]).

````tabs
tab: Julia
```julia
using Catlab
Ω, subs = subobject_classifier(Graph)
Ω                    # Graph: V = 1:2, E = 1:5, src = [1,1,1,2,2], tgt = [1,1,2,1,2]
                     # vertex 1 = V ("present"), vertex 2 = 0 ("absent");
                     # edges: 1 = (V,V;A), 2 = (V,V;0), 3 = (V,0;0), 4 = (0,V;0), 5 = (0,0;0)
subs[:E]             # the five subobjects of the representable edge that name these arrows
T = ob(terminal(Graph))                       # one vertex, one loop
# classify a subgraph: the unique hom G → Ω pulling `true` (loop ↦ edge 1) back to H
G = path_graph(Graph, 3); H = Subobject(G, V=[1, 2], E=[1])
χ = ACSetTransformation(G, Ω; V=[1, 1, 2], E=[1, 3])   # ⌜H⌝: vertices 1,2 present, 3 absent
is_natural(χ)                                 # true
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
-- presheaf categories are toposes; Mathlib has the classifier for `Type` and general sheaf machinery
#check @CategoryTheory.Functor                 -- Grph ≃ (ArShpᵒᵖ ⥤ Type)
#check @CategoryTheory.Subobject               -- Sub(G) for a presheaf G
```
tab: Haskell
```haskell
-- the five "truth values" for an arrow of a graph, and the two for a vertex
data OmegaV = Absent | Present deriving (Eq, Show)
data OmegaE = E00 | E0V | EV0 | EVV0 | EVVA deriving (Eq, Show)
classifyEdge :: Bool -> Bool -> Bool -> OmegaE      -- (source in H, target in H, edge in H)
classifyEdge True  True  True  = EVVA
classifyEdge True  True  False = EVV0
classifyEdge True  False _     = EV0
classifyEdge False True  _     = E0V
classifyEdge False False _     = E00
```
````
