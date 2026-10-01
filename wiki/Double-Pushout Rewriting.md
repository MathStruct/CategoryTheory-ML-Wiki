#definition #theorem #example #program

**Double-pushout (DPO) rewriting** is the categorical definition of "find a pattern in a graph and replace it". A **rule** is a [[Span]]

$$
L \xleftarrow{\;l\;} I \xrightarrow{\;r\;} R
$$

in a category of graph-like objects (graphs, hypergraphs, [[C-Set|C-sets]]): $L$ is the pattern, $R$ the replacement, and the **interface** $I$ is what is preserved, with $l$ and $r$ saying where it sits in each. A **match** is a morphism $m : L \to G$. Applying the rule is two squares, both [[Pushout|pushouts]]:

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}[column sep=large, row sep=large]
L \arrow[d, "m"'] & I \arrow[l, "l"'] \arrow[r, "r"] \arrow[d] & R \arrow[d] \\
G & K \arrow[l] \arrow[r] & H
\end{tikzcd}
\end{document}
```

First find $K$ — "$G$ with $L \setminus I$ deleted" — such that the left square is a pushout (a **pushout complement**); then glue in $R$ along $I$ by an ordinary pushout to get the result $H$. Deletion is the inverse of gluing, which is why both steps are pushouts.

> Sources: Ehrig, Pfender & Schneider, *Graph-grammars: an algebraic approach*, SWAT 1973 (the original DPO); Lack & Sobociński, *Adhesive categories*, FoSSaCS 2004 (the axiomatic setting); Brown, Patterson, Hanks & Fairbanks, *Computational Category-Theoretic Rewriting*, ICGT 2022, [arXiv:2111.03784](https://arxiv.org/abs/2111.03784) ([[Computational Category-Theoretic Rewriting|notes]]) §3 (pushout complements in C-sets, identification and dangling conditions, DPO/SPO/SqPO/PBPO+); Bonchi, Gadducci, Kissinger, Sobociński & Zanasi, *Rewriting modulo symmetric monoidal structure*, LICS 2016, [arXiv:1602.06771](https://arxiv.org/abs/1602.06771) ([[Rewriting Modulo Symmetric Monoidal Structure|notes]]) Definitions 3.2, 4.1, 5.5, Theorems 3.3, 4.4, 4.6, 5.6, Proposition 4.7.

## When the pushout complement exists

Pushouts always exist in a category of C-sets, but pushout complements need not, and need not be unique. For C-sets, with $l$ monic, a complement exists exactly when two **gluing conditions** hold (Brown et al., §3.0.1):

- **identification**: $m$ may only identify two elements of $L$ if both are preserved (in the image of $l$) — you cannot delete something and keep it at the same time;
- **dangling**: if $m$ deletes an element, it must also delete everything in $G$ that refers to it — no edge may be left without its source or target.

When both hold and $l$ is monic, the complement is unique. The categories where all of this behaves — pushouts along monos are stable and are also pullbacks — are the **adhesive categories** of Lack and Sobociński; every category of C-sets ([[Presheaf|presheaves]]) is adhesive, which is why one implementation covers graphs, hypergraphs, Petri nets and wiring diagrams at once. Variants weaken or replace the left square: SPO (single pushout, deletes dangling edges), SqPO (sesqui-pushout, a final pullback complement — allows cloning) and PBPO+ (pullback–pushout).

## String diagrams are hypergraphs, so equational reasoning is DPO

Bonchi et al. connect this to algebra. A string diagram over a signature $\Sigma$ is a hypergraph with interfaces — a [[Cospan]] of hypergraphs — and the PROP of these "Frobenius termgraphs" is exactly the free symmetric monoidal theory on $\Sigma$ plus a [[Frobenius Monoid]] (Theorem 3.3). Rewriting with equations of a symmetric monoidal theory is DPO rewriting of those hypergraphs (Theorem 4.4, Theorem 4.6), and the category of hypergraphs is adhesive (Proposition 4.7). Without Frobenius structure — for plain symmetric monoidal categories — one must restrict to **convex** matches and convex DPO steps (Definition 5.5, Theorem 5.6), so that a rewrite cannot create a cycle the theory does not have.

This is the mathematics behind graph IRs. A sea-of-nodes or dataflow IR is a hypergraph whose wires are values and whose hyperedges are operations; a peephole optimisation is a DPO rule; the gluing conditions are the checks that the optimisation does not leave a use without a definition.

## Relation to term rewriting and e-graphs

| | term rewriting | DPO graph rewriting | [[E-Graph|equality saturation]] |
|---|---|---|---|
| object | a tree | a graph / C-set | an e-graph (a set of terms up to congruence) |
| step | replace a subterm | pushout complement, then pushout | add right-hand side, merge classes |
| sharing | none (trees) | explicit, preserved by $I$ | maximal (hashcons) |
| destructive? | yes | yes — $L \setminus I$ is deleted | no — nothing is ever deleted |

DPO rewriting is the right model when the object being rewritten *is* the graph (an IR, a database instance). Equality saturation is the right model when one wants to keep every equivalent version and choose later.

## Sophia

Sophia's store is append-only, so it never performs the deletion half of a DPO step on stored nodes: a rewrite adds the right-hand side and an `EQUIV` edge, as in an e-graph. DPO is the semantics of the *lowering* steps on graph IRs (MLIR regions, LLVM basic blocks), and of schema migrations of the store itself, where the graph genuinely changes. See [Compilation as Query](https://mathstruct.org/Sophia/vault/Design/Compilation-as-Query) and [Graph Schema](https://mathstruct.org/Sophia/vault/Design/Graph-Schema).

````tabs
tab: Julia
**Docs:** [AlgebraicRewriting.jl](https://algebraicjulia.github.io/AlgebraicRewriting.jl/stable/) · [C-set morphisms](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.CSets) · [Limits & colimits](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.Limits) · [Graphs](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/graphs/)
```julia
using Catlab, AlgebraicRewriting
# Rule L ← I → R on graphs: replace a two-step path a→b→c by a direct edge a→c (b is kept).
L = path_graph(Graph, 3)                     # 1 → 2 → 3
I = Graph(3)                                 # the interface: three vertices, no edges
R = Graph(3); add_edge!(R, 1, 3)             # 1 → 3
l = ACSetTransformation(I, L; V = [1, 2, 3], E = Int[])
r = ACSetTransformation(I, R; V = [1, 2, 3], E = Int[])
rule = Rule(l, r)
G = path_graph(Graph, 4)                     # 1 → 2 → 3 → 4
H = rewrite(rule, G)                         # DPO: pushout complement, then pushout
(nv(H), ne(H))                               # (4, 2): two path edges removed, one shortcut added
sort([(src(H, e), tgt(H, e)) for e in edges(H)])   # [(1, 3), (3, 4)]: the first match a=1,b=2,c=3 was used
# The dangling condition: deleting a vertex with an unmatched incident edge is not allowed.
Ldel = Graph(1); Idel = Graph(0)
kill = Rule(ACSetTransformation(Idel, Ldel; V = Int[]), id(Idel))
m = ACSetTransformation(Ldel, G; V = [2])    # match vertex 2 of G, which has edges in and out
can_match(kill, m)                           # ("Gluing conditions failed", … "Dangling" …): edges 1→2, 2→3 would dangle
```
tab: Lean
```lean
import Mathlib
open CategoryTheory Limits
-- The second square of a DPO step is an ordinary pushout; Mathlib states pushout squares as `IsPushout`.
-- Pushouts compose (pasting), which is why a sequence of rewrites is again a pushout diagram.
#check @IsPushout
#check @IsPushout.paste_horiz
-- A rule is a span L ← I → R; a match is a morphism L ⟶ G.
structure DPORule (C : Type*) [Category C] where
  L : C
  I : C
  R : C
  l : I ⟶ L
  r : I ⟶ R
  mono_l : Mono l      -- left-linear rules: the pushout complement is unique when it exists
```
````
