#annotation #program

**Catlab.jl** is the Julia library for applied category theory at the heart of the AlgebraicJulia ecosystem: generalized algebraic theories (GATs) via `@theory`, symbolic presentations via `@present`, [[Category of Finite Sets|finite sets and functions]] (`FinSet`, `FinFunction`), [[C-Set|ACSets]] (`@acset_type`, `@acset`), [[Limit|limits]] and [[Colimit|colimits]], [[Wiring Diagram|wiring diagrams]], and graphics.

> Sources: Kittenlab (which builds a toy version of Catlab step by step); AlgebraicJulia documentation.

**Version note.** All Julia code in this wiki was checked against **Catlab v0.16.x** (the API used by the current documentation and by Kittenlab). Catlab v0.17 moved to GATlab-style explicit models (`meet[SubobjectElementWise()](U, V)` instead of `meet(U, V)`, etc.); most `FinSet`/`FinFunction`/ACSet code is unchanged, but lattice operations on subobjects and some theory-level calls differ.

## Running the Julia tabs

```julia
using Pkg
Pkg.add(name = "Catlab", version = "0.16")          # the version the snippets were run against
Pkg.add(url = "https://github.com/AlgebraicJulia/Kittenlab.jl")   # optional: Kittenlab's own package
Pkg.add(name = "AlgebraicRewriting", version = "0.4")   # graph rewriting (Double-Pushout Rewriting note)
```

Every Julia tab in the vault begins with a **Docs:** line pointing at the relevant section of the [Catlab v0.16 documentation](https://algebraicjulia.github.io/Catlab.jl/v0.16/) (e.g. [FinSets](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FinSets), [Limits](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.Limits), [CSets](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.CSets), [data migration](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FunctorialDataMigrations), [graphs](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/graphs/), [wiring diagrams](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/wiring_diagrams/)), the [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/) (theories such as `ThCategory`), the [ACSets.jl API](https://algebraicjulia.github.io/ACSets.jl/stable/api/), the [Catlab vignettes](https://algebraicjulia.github.io/Catlab.jl/v0.16/generated/sketches/preorders/) that follow 7 Sketches, or the [Kittenlab lectures](https://algebraicjulia.github.io/Kittenlab.jl/lecture1.html). Notes whose code is plain Julia say so. The current Catlab release (v0.17+) is documented at [the stable docs](https://algebraicjulia.github.io/Catlab.jl/stable/); for most `FinSet`/ACSet code the v0.16 pages still describe the same functions.

Snippets are self-contained unless a **Builds on:** line says otherwise. The Kittenlab-style mini-library is spread over a few notes, and the chains are:

- [[Category]] → [[Functor]] (via [[Prop of Matrices]]) → [[Natural Transformation]] → [[Functor Category]], [[Category of Categories]]
- [[Preorder]] → [[Bool (Monoidal Preorder)]], [[Cost]], [[Natural Numbers]] → [[Monotone Map]], [[Enriched Category]] → [[Matrix Multiplication in a Quantale]], [[Lawvere Metric Space]] → [[Profunctor]] → [[Category of Profunctors]], [[Collage]]
- [[Finite Set]] → [[Function]] → [[Function Composition]], [[Isomorphism]], [[Identity Function]], [[Cardinality]]

Snippets that start with `using Catlab` shadow the mini-library's `Category`, `Functor`, `FinFunction`, …, so run them in a fresh session (or module).

Kittenlab's own mini-library (`src/Categories.jl`, `FinSets.jl`, `Functors.jl`, `NaturalTransformations.jl`, `FinCats.jl`, `Diagrams.jl`, `Graphs.jl`) is reproduced across [[Category]], [[Functor]], [[Natural Transformation]], [[Presentation of a Category]], [[Diagram]] and [[Graph]]; it takes a "middle path": Julia types guide implementation and dispatch but are not relied on for correctness.

## Where to look

| Concept | Catlab |
|---|---|
| [[Category]] | `Catlab.Theories.ThCategory`, `FreeCategory` |
| [[Preorder]] | `ThPreorder`, `FreePreorder`, `ThThinCategory` |
| [[Symmetric Monoidal Category]] | `ThSymmetricMonoidalCategory`, `FreeSymmetricMonoidalCategory` |
| [[Category of Finite Sets]] | `FinSet`, `FinFunction`, `Catlab.CategoricalAlgebra.FinSets` |
| [[Category of Relations]] | `Catlab.CategoricalAlgebra.FinRelations` |
| [[Presentation of a Category]] / [[Database Schema]] / [[Categories and Schemas are Equivalent\|schemas]] | `@present`, `FreeSchema`, `FinCat` |
| [[C-Set]] / [[Functor]] $\mathcal{C} \to \mathbf{Set}$ | `@acset_type`, `@acset`, `FinDomFunctor` |
| [[Natural Transformation]] / [[Graph Homomorphism]] | `ACSetTransformation`, `homomorphism(s)`, `isomorphisms`, `is_natural` |
| [[Graph]], [[Symmetric Graph]] | `Graph`, `SymmetricGraph`, `path_graph`, `cycle_graph` |
| [[Monoid Action]], [[Finite State Machine]], [[Discrete Dynamical System]] | ACSets on a one-object schema (`@acset_type`) |
| [[Limit]], [[Colimit]], [[Pushout]], [[Pullback]] | `limit`, `colimit`, `pushout`, `pullback`, `coequalizer` |
| [[Data Migration Functor\|Data migration]] $\Delta_F, \Sigma_F, \Pi_F$ | `DeltaMigration`, `SigmaMigration`, `migrate` |
| [[Cospan]], [[Decorated Cospan]], [[Structured Cospan]] | `Cospan`, `StructuredCospan`, `OpenCSet` |
| [[Hypergraph Category]] / [[Wiring Diagram\|UWDs]] | `@relation`, `oapply`, `UndirectedWiringDiagram` |
| [[Prop]] / [[Signal Flow Graph]] | `ThBiproductCategory`, `@theory`, `Catlab.Programs` |
| [[Operad]] | `oapply` (operad algebras), `Catlab.WiringDiagrams` |
| [[Subobject]] lattice | `Subobject`, `meet`, `join`, `top`, `bottom` |
