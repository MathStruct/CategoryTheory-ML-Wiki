#theorem #definition #example

A [[Database Schema|schema]] is a [[Graph]] together with a congruence of path equivalence declarations (PEDs) — a [[Presentation of a Category]]. Category Theory for Scientists defines a **schema morphism** $F : \mathcal C \to \mathcal C'$ as a [[Graph Homomorphism]] $G \to \mathrm{Paths}(G')$ that sends equivalent paths to equivalent paths:

> **Slogan (CTfS 4.4.1.3).** *Vertices go to vertices, arrows go to paths, and path equivalences go to path equivalences.*

Schemas and schema morphisms form a category $\mathbf{Sch}$ (composition is the composition in the [[Kleisli Category]] of the $\mathrm{Paths}$ [[Monad]] on $\mathbf{Grph}$, CTfS Remark 5.3.2.7), and

> **Theorem (CTfS 4.4.2.3).** The functors $L : \mathbf{Sch} \to \mathbf{Cat}$ and $R : \mathbf{Cat} \to \mathbf{Sch}$ form an [[Equivalence of Categories]] $\mathbf{Sch} \simeq \mathbf{Cat}$.

> Sources: CTfS §4.4 (Exercise 4.4.1.1, Definition 4.4.1.2, Slogan 4.4.1.3, Examples 4.4.1.4, Exercises 4.4.1.5–4.4.1.7, Constructions 4.4.2.1–4.4.2.2, Theorem 4.4.2.3), Slogan 4.2.2.1, Remark 5.3.2.7; 7 Sketches §3.2.2 (presenting categories via graphs and equations).

## The two functors

- $L$ (**the category presented**, CTfS Construction 4.4.2.1): objects are the vertices, morphisms are paths modulo the PEDs, composition is concatenation. This is the [[Free Category]] on the graph, quotiented.
- $R$ (**the tautological presentation**, CTfS Construction 4.4.2.2): the graph has a vertex per object and an arrow per morphism of $\mathcal C$, and a path is declared equivalent to the single arrow that is its composite.

$L(R(\mathcal C)) \cong \mathcal C$, but $R(L(\mathcal C))$ is a much bigger schema than $\mathcal C$ — it merely presents the same category. So the equivalence is *not* an isomorphism: the natural transformation $\mathrm{id} \Rightarrow R \circ L$ is an isomorphism in $\mathbf{Sch}$ (schema morphisms go both ways, since each arrow of $R L \mathcal C$ can be sent to a path in $\mathcal C$), not an equality of graphs. Two schemas are isomorphic in $\mathbf{Sch}$ iff they present isomorphic categories; this is why a schema is a good *finite* handle on a possibly infinite category, and why one can "think of categories and schemas as the same" (CTfS Slogan 4.2.2.1).

## Examples

- **An infinite category from a tiny schema.** The schema $\mathrm{Loop}$ with one vertex $s$, one arrow $f : s \to s$ and no PEDs presents $\mathbb N$: the paths are $\mathrm{id}_s, f, ff, fff, \dots$ (CTfS Exercise 4.2.2.2). A schema morphism $[2] \to \mathrm{Loop}$ sends both arrows of $0 \to 1 \to 2$ to paths, for example $f_1 \mapsto f$, $f_2 \mapsto ff$ (CTfS Exercise 4.4.1.1).
- **A commutative triangle** (CTfS Example 4.4.1.4): the linear order $[2]$ presented by $0 \xrightarrow{f_1} 1 \xrightarrow{f_2} 2$ is isomorphic in $\mathbf{Sch}$ to the schema $\mathcal C$ with arrows $g : a \to b$, $h : b \to c$, $i : a \to c$ and PED $[g, h] \simeq [i]$ — send $i$ to the path $f_1 f_2$, and back send $f_1 \mapsto g$, $f_2 \mapsto h$.
- **Counting** ([[CTfS Chapter 4 Exercises#Exercise 4.4.1.5|CTfS Exercise 4.4.1.5]]): if $\mathcal C$ has *no* PED, schema morphisms $[2] \to \mathcal C$ with $0 \mapsto a$ number 8 (choose where $1$ and $2$ go and a path for each arrow; the two paths $gh$ and $i$ from $a$ to $c$ are now different), and morphisms $\mathcal C \to [2]$ with $a \mapsto 0$ number 6 (the target is thin, so a morphism is just a choice of $b \mapsto \beta$, $c \mapsto \gamma$ with $0 \leq \beta \leq \gamma$: $(\beta, \gamma) \in \{(0,0), (0,1), (0,2), (1,1), (1,2), (2,2)\}$).
- **Idempotent-ish loops** ([[CTfS Chapter 4 Exercises#Exercise 4.4.1.6|CTfS Exercises 4.4.1.6]]–[[CTfS Chapter 4 Exercises#Exercise 4.4.1.7|4.4.1.7]]): $L_n$ is $\mathrm{Loop}$ with the PED $f^{n+1} = f^n$ — the cyclic monoid $C_{n,1}$ of [[Presentation of a Monoid]] with $n+1$ elements. $L_0 \cong \mathbf 1$ (there $f = \mathrm{id}$) while $L_1$ has the two morphisms $\mathrm{id}, f$ with $f^2 = f$. A morphism $L_m \to L_n$ sends $f \mapsto f^k$ and must respect the PED, which in $L_n$ forces $k = 0$ or $km \geq n$; so $|\mathrm{Hom}(L_3, L_5)| = 5$ ($k = 0, 2, 3, 4, 5$) and $|\mathrm{Hom}(L_5, L_3)| = 4$ ($k = 0, 1, 2, 3$). CTfS's hint $|\mathrm{Hom}(L_4, L_9)| = 8$ checks out: $k = 0$ and $k = 3, \dots, 9$.

## Why it matters

Functors out of $L(\mathcal C)$ are determined by where the generating arrows go, subject to the PEDs — this is how instances ([[C-Set]]s), [[Functor|functors]] between presented categories and [[Data Migration Functor|data migration]] are specified in practice, in CTfS and in Catlab's `@present`/`FinFunctor`. The same "presentation vs. presented thing" split occurs for [[Presentation of a Monoid|monoids]], [[Presentation of a Prop|props]] and [[Reflexive Transitive Closure|preorders]].

````tabs
tab: Julia
**Docs:** [FinCats](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FinCats) · [Theories & presentations](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/theories/)
```julia
using Catlab
# A schema is a presentation; the category it presents may be infinite (Loop ↦ ℕ)
@present SchFather(FreeSchema) begin
  (F, C)::Ob
  c::Hom(F, C)       # "a father has as first child a child"
  f::Hom(C, F)       # "a child has as father a father"
  c ⋅ f == id(F)     # the father's first child's father is the father (CTfS Exercise 3.5.2.18)
end
FC = FinCat(SchFather)
ob_generators(FC), hom_generators(FC)        # the graph of the schema: 2 vertices, 2 arrows
# Schema morphisms L_m → L_n are f ↦ fᵏ respecting f^(m+1) = f^m (CTfS Exercise 4.4.1.7)
normal(a, n) = min(a, n)                      # in L_n, fᵃ = fᵇ iff min(a,n) = min(b,n)
homs(m, n) = [k for k in 0:n if normal(k * (m + 1), n) == normal(k * m, n)]
homs(3, 5), homs(5, 3), length(homs(4, 9))   # ([0, 2, 3, 4, 5], [0, 1, 2, 3], 8)
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
-- the free category on a quiver (paths) and quotients by a relation on morphisms:
#check @Paths                      -- the path category of a quiver
#check @Quotient.functor           -- C ⥤ Quotient r: impose path equations (PEDs)
#check @Quiver.Path                -- paths in a graph
-- "arrows go to paths": a prefunctor V ⥤q Paths W extends to a functor Paths V ⥤ Paths W
#check @Paths.lift
```
tab: Haskell
```haskell
-- the schema L_n: one object, one arrow f with f^(n+1) = f^n;
-- morphisms are exponents, normalised by min · n
normal :: Int -> Int -> Int
normal n a = min a n

-- schema morphisms L_m → L_n: f ↦ f^k preserving the PED
homs :: Int -> Int -> [Int]
homs m n = [ k | k <- [0..n], normal n (k * (m + 1)) == normal n (k * m) ]
-- homs 3 5 == [0,2,3,4,5];  homs 5 3 == [0,1,2,3];  length (homs 4 9) == 8
```
````
