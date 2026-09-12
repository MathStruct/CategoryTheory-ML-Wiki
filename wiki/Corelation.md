#definition #example

Given finite sets $A$ and $B$, a **corelation** $A \to B$ is an [[Equivalence Relation]] on $A \sqcup B$ (drawn as dashed loops encircling equivalent elements). **$\mathbf{Corel}$** is the [[Category]] with finite sets as objects and corelations as morphisms; the composite $\beta \circ \alpha$ of $\alpha : A \to B$ and $\beta : B \to C$ relates two elements of $A \sqcup C$ iff one can travel from one to the other staying within equivalence classes of $\alpha$ or $\beta$. Formally: $\alpha \mathbin{;} \beta := \iota_{A \sqcup C}^*\big((\iota_{A \sqcup B})_!(\alpha) \vee (\iota_{B \sqcup C})_!(\beta)\big)$ — push both relations forward to $A \sqcup B \sqcup C$, take the join (transitive closure of the union) in the preorder of equivalence relations, and pull back to $A \sqcup C$ (using the [[Pushforward and Pullback of Partitions|pushforward and pullback]] of §1.4).

> Sources: 7 Sketches Example 4.61, Exercise 4.62, footnote 2; §6.x (corelations as a [[Hypergraph Category]], "we'll see it again in the chapters to come").

$\mathbf{Corel}$ is a [[Symmetric Monoidal Category]] under $(\varnothing, \sqcup)$ and is **[[Compact Closed Category|compact closed]] with every finite set its own dual**: the unit $\eta_A : \varnothing \to A \sqcup A$ and the counit $\varepsilon_A : A \sqcup A \to \varnothing$ are both the equivalence relation on $A \sqcup A$ whose parts are the pairs $\{(a, 1), (a, 2)\}$; the snake equations hold because composing two such pairings along the middle copy of $A$ yields the pairing again ([[7S Exercise 4.62]] for $\underline{3}$). Corelations are the "connectivity" of [[Cospan|cospans]] of finite sets (a cospan $A \to N \leftarrow B$ induces the equivalence "same image in $N$"), and $\mathbf{Corel}$ is the prototypical [[Hypergraph Category]] used for electrical circuits: a corelation says which terminals are wired together.

````tabs
tab: Julia
```julia
# corelations as partitions (equivalence relations) of A ⊔ B, composed by union-find on A ⊔ B ⊔ C
using DataStructures
struct Corel; nA::Int; nB::Int; classes::Vector{Vector{Int}}; end   # elements 1..nA are A, nA+1..nA+nB are B
function compose(α::Corel, β::Corel)
  n = α.nA + α.nB + β.nB
  uf = IntDisjointSets(n)
  for cl in α.classes, (x, y) in zip(cl, cl[2:end]); union!(uf, x, y); end
  for cl in β.classes, (x, y) in zip(cl .+ α.nA, cl[2:end] .+ α.nA); union!(uf, x, y); end
  keep = [1:α.nA; α.nA + α.nB + 1 : n]                               # restrict to A ⊔ C
  roots = Dict(); for x in keep; push!(get!(roots, find_root!(uf, x), Int[]), x); end
  relabel = Dict(x => i for (i, x) in enumerate(keep))
  Corel(α.nA, β.nB, [relabel[x] |> (i -> i) for cl in values(roots) for x in [cl]] |> identity)  # sketch
end
# Catlab: `Catlab.CategoricalAlgebra.FinRelations` and hypergraph-category machinery cover corelations via cospans
```
tab: Haskell
```haskell
-- a corelation A -> B as a partition of Either a b; composition = join of partitions then restrict
type Corel a b = [[Either a b]]
```
````
