#definition #example

A **span** in a [[Category]] $\mathcal{C}$ from $X$ to $Y$ is a [[Diagram]] $X \xleftarrow{f} A \xrightarrow{g} Y$, i.e. a [[Cone]] over the discrete diagram $\{X, Y\}$. Its [[Limit]] is the [[Product]] (spans are "objects equipped with morphisms to $X$ and $Y$", 7 Sketches §3.5.2); the [[Colimit]] of a span is a [[Pushout]].

> Sources: 7 Sketches §3.5.2, Exercise 3.91, §4.5 ([[Compact Closed Category]]); DaoFP §9.4 ("Product as a universal span"); Kittenlab Lecture 15 ("One way of thinking about a relation $R \subseteq X \times Y$ is that it is a span"; "syntax and semantics are dual" — spans for semantics, cospans for syntax); CTfS §2.5.2 (Definition 2.5.2.1, Applications 2.5.2.2, 2.5.2.4, Definition 2.5.2.3, Construction 2.5.2.5, Exercise 2.5.2.6), Example 3.3.1.6

- **Experiments are spans** (CTfS Application 2.5.2.2). A set $E$ of experiments recording the temperature $T$ and the pressure $P$ of a gas is a span $T \xleftarrow{f} E \xrightarrow{g} P$ — a table with columns ID, Temperature, Pressure (100 → 72, 100 → 73, 100 → 72, 200 → 140, …). Several experiments may give the same pair, and some temperatures none: not a function, not even a relation.
- **Composing data sources** (CTfS Application 2.5.2.4): if an online lab publishes a span $P \leftarrow E' \to V$ (pressure vs. container volume), the fiber product $E \times_P E'$ is a span $T \leftarrow E'' \to V$: "whenever an experiment in our lab yielded the same pressure as one they recorded, call that a data point". Unscientific, perhaps — but reproducible and fully transparent.
- **Spans categorify matrices** (CTfS §2.5.2): a span $A \leftarrow R \to B$ of finite sets gives the $\mathbb N$-matrix whose $(a,b)$ entry counts the elements of $R$ over $(a,b)$; disjoint union of spans adds matrices and composition by pullback multiplies them. Drawn as a bipartite graph, $R$ is the set of edges (CTfS Construction 2.5.2.5, Example 3.3.1.6).
- A [[Relation]] $R \subseteq X \times Y$ is a jointly monic span $X \leftarrow R \to Y$; spans in $\mathbf{Set}$ compose by [[Pullback]], giving the category (bicategory) of spans, which for jointly-monic spans is the [[Category of Relations]].
- Spans of graphs / $\mathcal{C}$-sets are the *rewrite rules* of double-pushout rewriting (Catlab); spans with a [[Product]] apex are how [[Profunctor|profunctors]] and [[Feasibility Relation|feasibility relations]] arise.
- Dual: [[Cospan]] (composition by pushout; [[Undirected Wiring Diagram|undirected wiring diagrams]]).

````tabs
tab: Julia
**Docs:** [FinSets](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FinSets) · [Limits & colimits](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.Limits) · [Free diagrams](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FreeDiagrams) — Kittenlab [Lecture 15](https://algebraicjulia.github.io/Kittenlab.jl/lecture15.html)
```julia
using Catlab
s = Span(FinFunction([1, 2, 2], 3), FinFunction([1, 1, 2], 2))   # apex FinSet(3), legs to 3 and 2
apex(s), legs(s)
# composition of spans by pullback (done by hand in Catlab 0.16):
t = Span(FinFunction([1, 2], 2), FinFunction([2, 1], 2))
pb = pullback(right(s), left(t))
st = Span(compose(legs(pb)[1], left(s)), compose(legs(pb)[2], right(t)))
apex(st)                       # FinSet(3)
```
tab: Lean
```lean
#check CategoryTheory.Limits.span        -- span f g : WalkingSpan ⥤ C
#check CategoryTheory.Limits.WalkingSpan
```
tab: Haskell
```haskell
data Span a x y = Span (a -> x) (a -> y)    -- with apex a
-- a relation as a span: the apex is the set of related pairs
relSpan :: [(x, y)] -> Span (x, y) x y
relSpan _ = Span fst snd
```
````
