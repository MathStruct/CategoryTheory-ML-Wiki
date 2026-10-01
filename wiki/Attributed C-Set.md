#definition #theorem #example #program

An **attributed C-set** (**acset**) is a [[C-Set]] whose elements may also carry *data* — numbers, strings, labels — drawn from fixed sets that morphisms must not change. Following Patterson, Lynch & Fairbanks:

- a **schema** (Definition 5) is a small category $|S|$ with a functor $S : |S| \to \mathbf 2$ to the walking arrow $\{0 \to 1\}$; objects over $0$ are **combinatorial** objects (tables: `Term`, `Edge`), objects over $1$ are **attribute types** (`Label`, `Int`), and arrows from a table to an attribute type are **attributes**;
- given a **typing** $K : S_1 \to \mathbf{Set}$ that fixes a set for each attribute type, an **acset** (Definition 6) is a functor $X : |S| \to \mathbf{Set}$ that restricts to $K$ on $S_1$, and a **morphism of acsets** is a natural transformation that is the *identity* on the attribute types.

So the combinatorial part is free to map, glue and quotient as in any C-set, while the attribute values are fixed: a homomorphism must send a node labelled `:mul` to a node labelled `:mul`. This is the data structure underneath Catlab: graphs, Petri nets, wiring diagrams and the e-graph-like term graphs of this note are all acsets on different schemas.

> Sources: Patterson, Lynch & Fairbanks, *Categorical Data Structures for Technical Computing*, Compositionality 4 (2022), [arXiv:2106.04703](https://arxiv.org/abs/2106.04703) ([[Categorical Data Structures for Technical Computing|notes]]) Definitions 1–9, Proposition 1, Theorem 2, Propositions 3, 5, Corollary 6; Spivak, *Functorial data migration*, Inform. and Comput. 217 (2012) and CTfS §§3.5, 4.4 for C-sets as database instances; Schultz, Spivak, Vasilakopoulou & Wisnesky [arXiv:1602.03501](https://arxiv.org/abs/1602.03501) ([[Algebraic Databases|notes]]) for the version with an algebraic type side ([[Algebraic Database]]); Kittenlab Lecture 6 (functors as data structures).

## Acsets are a slice category

Fixing attribute values looks like an extra condition, but it is a familiar construction in disguise:

> **Theorem 2.** For a schema $S$ and typing $K$, the category $\mathbf{Acset}^S_K$ is isomorphic to a [[Slice Category]] $\mathbf{Set}^{\mathcal C}/D$ for a C-set $D$ built from $K$ by a [[Kan Extension]].

Everything known about C-sets transfers. Limits and colimits are computed pointwise (Proposition 1) and lift through the slice (Propositions 3, 5); acsets have all finite limits (Corollary 6) and the colimits that respect attributes. Hence Catlab's generic `limit`, `colimit`, `homomorphism`, [[Data Migration Functor|data migration]] and [[Double-Pushout Rewriting|DPO rewriting]] work for every schema without schema-specific code — the point of the paper's title.

## Compositional data: structured cospans of acsets

Gluing acsets along shared parts is a pushout, so open systems built from acsets compose as [[Cospan|cospans]] (Definition 8) or [[Structured Cospan|structured cospans]] (Definition 9). Catlab uses this for open Petri nets, open graphs and wiring diagrams: one generic implementation of composition by pushout, instantiated by a schema.

## Acsets, property graphs and relational tables

| | relational tables | property graph | acset |
|---|---|---|---|
| schema | table definitions, foreign keys | node/edge labels (informal) | a finitely presented category $\lvert S \rvert \to \mathbf 2$ |
| integrity | foreign key constraints | none built in | functoriality: every foreign key lands in its table |
| path equations | triggers / checks | none | equations in the presentation |
| homomorphism | — | pattern matching | natural transformation fixing attributes |
| colimits | — | — | built in (gluing, quotienting) |

An acset is a relational database whose foreign keys are total and whose schema is a category; it is also a typed property graph whose type system is checked by functoriality.

## Sophia

Sophia's [Graph Schema](https://mathstruct.org/Sophia/vault/Design/Graph-Schema) is an acset schema: node kinds are tables, ordered `CHILD(i)` edges are a table with two foreign keys and a position attribute, hashes and payloads are attributes. Read this way, a frontend's output is an acset, an e-matching query is a [[Conjunctive Query]] on it, hash-schema migration is a [[Data Migration Functor]], and the backend trait for DuckDB / property graphs is a choice of how to store acsets. The Julia tab below builds a fragment of exactly that schema.

````tabs
tab: Julia
**Docs:** [C-set morphisms](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.CSets) · [ACSets API](https://algebraicjulia.github.io/ACSets.jl/stable/api/) · [Theories & presentations](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/theories/) — Kittenlab [Lecture 6](https://algebraicjulia.github.io/Kittenlab.jl/lecture6.html)
```julia
using Catlab
# A schema for a tiny code graph: term nodes, ordered child edges, and two attributes.
@present SchTermGraph(FreeSchema) begin
    (Term, Child)::Ob
    parent::Hom(Child, Term); child::Hom(Child, Term)
    (Label, Pos)::AttrType
    tag::Attr(Term, Label)      # node kind: the "tag" that is hashed
    ord::Attr(Child, Pos)       # argument position
end
@acset_type TermGraph(SchTermGraph, index = [:parent, :child])
# the term  add(x, mul(x, 2))  with x shared (a DAG, as after hash-consing)
t = @acset TermGraph{Symbol,Int} begin
    Term = 4; tag = [:add, :x, :mul, :lit2]
    Child = 4; parent = [1, 1, 3, 3]; child = [2, 3, 2, 4]; ord = [1, 2, 1, 2]
end
incident(t, 2, :child)                              # [1, 3]: x is used twice
[t[c, :ord] => t[t[c, :child], :tag] for c in incident(t, 1, :parent)]   # [1 => :x, 2 => :mul]
# A morphism of attributed C-sets must preserve the attributes: here, an embedding of the subterm mul(x, 2).
s = @acset TermGraph{Symbol,Int} begin
    Term = 3; tag = [:mul, :x, :lit2]
    Child = 2; parent = [1, 1]; child = [2, 3]; ord = [1, 2]
end
h = homomorphism(s, t)
collect(h[:Term])                                    # [3, 2, 4]: mul ↦ 3, x ↦ 2, lit2 ↦ 4
length(homomorphisms(s, t))                          # 1: tags and positions pin it down
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
-- A C-set is a functor C ⥤ Type; an acset additionally fixes the values on attribute objects.
-- Here: an attribute-preserving morphism is a natural transformation whose components at the
-- attribute objects are identities.
structure AcsetHom {C : Type*} [Category C] (attr : C → Prop) (X Y : C ⥤ Type) where
  app : X ⟶ Y
  fixes : ∀ c, attr c → ∀ h : X.obj c = Y.obj c, ∀ x, app.app c x = cast h x
#check @NatTrans.naturality
```
````
