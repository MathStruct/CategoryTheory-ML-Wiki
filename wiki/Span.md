#definition #example

A **span** in a [[Category]] $\mathcal{C}$ from $X$ to $Y$ is a [[Diagram]] $X \xleftarrow{f} A \xrightarrow{g} Y$, i.e. a [[Cone]] over the discrete diagram $\{X, Y\}$. Its [[Limit]] is the [[Product]] (spans are "objects equipped with morphisms to $X$ and $Y$", 7 Sketches §3.5.2); the [[Colimit]] of a span is a [[Pushout]].

> Sources: 7 Sketches §3.5.2, Exercise 3.91, §4.5 ([[Compact Closed Category]]); DaoFP §9.4 ("Product as a universal span"); Kittenlab Lecture 15 ("One way of thinking about a relation $R \subseteq X \times Y$ is that it is a span"; "syntax and semantics are dual" — spans for semantics, cospans for syntax).

- A [[Relation]] $R \subseteq X \times Y$ is a jointly monic span $X \leftarrow R \to Y$; spans in $\mathbf{Set}$ compose by [[Pullback]], giving the category (bicategory) of spans, which for jointly-monic spans is the [[Category of Relations]].
- Spans of graphs / $\mathcal{C}$-sets are the *rewrite rules* of double-pushout rewriting (Catlab); spans with a [[Product]] apex are how [[Profunctor|profunctors]] and [[Feasibility Relation|feasibility relations]] arise.
- Dual: [[Cospan]] (composition by pushout; [[Undirected Wiring Diagram|undirected wiring diagrams]]).

````tabs
tab: Julia
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
