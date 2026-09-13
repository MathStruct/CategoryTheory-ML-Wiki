#definition #example

Given finite sets $A$ and $B$, a **corelation** $A \to B$ is an [[Equivalence Relation]] on $A \sqcup B$ (drawn as dashed loops encircling equivalent elements). **$\mathbf{Corel}$** is the [[Category]] with finite sets as objects and corelations as morphisms; the composite $\beta \circ \alpha$ of $\alpha : A \to B$ and $\beta : B \to C$ relates two elements of $A \sqcup C$ iff one can travel from one to the other staying within equivalence classes of $\alpha$ or $\beta$. Formally: $\alpha \mathbin{;} \beta := \iota_{A \sqcup C}^*\big((\iota_{A \sqcup B})_!(\alpha) \vee (\iota_{B \sqcup C})_!(\beta)\big)$ — push both relations forward to $A \sqcup B \sqcup C$, take the join (transitive closure of the union) in the preorder of equivalence relations, and pull back to $A \sqcup C$ (using the [[Pushforward and Pullback of Partitions|pushforward and pullback]] of §1.4).

> Sources: 7 Sketches Example 4.61, Exercise 4.62, footnote 2; §6.x (corelations as a [[Hypergraph Category]], "we'll see it again in the chapters to come").

$\mathbf{Corel}$ is a [[Symmetric Monoidal Category]] under $(\varnothing, \sqcup)$ and is **[[Compact Closed Category|compact closed]] with every finite set its own dual**: the unit $\eta_A : \varnothing \to A \sqcup A$ and the counit $\varepsilon_A : A \sqcup A \to \varnothing$ are both the equivalence relation on $A \sqcup A$ whose parts are the pairs $\{(a, 1), (a, 2)\}$; the snake equations hold because composing two such pairings along the middle copy of $A$ yields the pairing again ([[7S Chapter 4 Exercises#Exercise 4.62|7S Exercise 4.62]] for $\underline{3}$). Corelations are the "connectivity" of [[Cospan|cospans]] of finite sets (a cospan $A \to N \leftarrow B$ induces the equivalence "same image in $N$"), and $\mathbf{Corel}$ is the prototypical [[Hypergraph Category]] used for electrical circuits: a corelation says which terminals are wired together.

````tabs
tab: Julia
```julia
# corelations A → B as equivalence relations on A ⊔ B (elements 1..nA are A, nA+1..nA+nB are B),
# composed by union-find on A ⊔ B ⊔ C and restriction to A ⊔ C
using DataStructures
struct Corel
  nA::Int; nB::Int
  pairs::Vector{Tuple{Int,Int}}       # generating pairs of the equivalence relation
end
function compose(α::Corel, β::Corel)
  n = α.nA + α.nB + β.nB
  uf = IntDisjointSets(n)
  for (x, y) in α.pairs; union!(uf, x, y); end
  for (x, y) in β.pairs; union!(uf, x + α.nA, y + α.nA); end       # B ⊔ C sits after A
  keep = vcat(1:α.nA, α.nA + α.nB + 1 : n)                          # restrict to A ⊔ C
  relabel = Dict(x => i for (i, x) in enumerate(keep))
  pairs = [(relabel[x], relabel[y]) for x in keep for y in keep if x < y && in_same_set(uf, x, y)]
  Corel(α.nA, β.nB, pairs)
end
# the unit/counit corelation on A = 3: pair (a,1) with (a,2)
η3 = Corel(0, 6, [(1, 4), (2, 5), (3, 6)])       # ∅ → 3 ⊔ 3
ε3 = Corel(6, 0, [(1, 4), (2, 5), (3, 6)])       # 3 ⊔ 3 → ∅
```
tab: Haskell
```haskell
-- a corelation A -> B as a partition of Either a b; composition = join of partitions then restrict
type Corel a b = [[Either a b]]
```
````
