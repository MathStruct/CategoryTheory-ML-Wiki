#annotation #program

**Catlab.jl** is the Julia library for applied category theory at the heart of the AlgebraicJulia ecosystem: generalized algebraic theories (GATs) via `@theory`, symbolic presentations via `@present`, [[Category of Finite Sets|finite sets and functions]] (`FinSet`, `FinFunction`), [[C-Set|ACSets]] (`@acset_type`, `@acset`), [[Limit|limits]] and [[Colimit|colimits]], [[Wiring Diagram|wiring diagrams]], and graphics.

> Sources: Kittenlab (which builds a toy version of Catlab step by step); AlgebraicJulia documentation.

**Version note.** All Julia code in this wiki was checked against **Catlab v0.16.x** (the API used by the current documentation and by Kittenlab). Catlab v0.17 moved to GATlab-style explicit models (`meet[SubobjectElementWise()](U, V)` instead of `meet(U, V)`, etc.); most `FinSet`/`FinFunction`/ACSet code is unchanged, but lattice operations on subobjects and some theory-level calls differ.

Kittenlab's own mini-library (`src/Categories.jl`, `FinSets.jl`, `Functors.jl`, `NaturalTransformations.jl`, `FinCats.jl`, `Diagrams.jl`, `Graphs.jl`) is reproduced across [[Category]], [[Functor]], [[Natural Transformation]], [[Presentation of a Category]], [[Diagram]] and [[Graph]]; it takes a "middle path": Julia types guide implementation and dispatch but are not relied on for correctness.

## Where to look

| Concept | Catlab |
|---|---|
| [[Category]] | `Catlab.Theories.ThCategory`, `FreeCategory` |
| [[Preorder]] | `ThPreorder`, `FreePreorder`, `ThThinCategory` |
| [[Symmetric Monoidal Category]] | `ThSymmetricMonoidalCategory`, `FreeSymmetricMonoidalCategory` |
| [[Category of Finite Sets]] | `FinSet`, `FinFunction`, `Catlab.CategoricalAlgebra.FinSets` |
| [[Category of Relations]] | `Catlab.CategoricalAlgebra.FinRelations` |
| [[Presentation of a Category]] / [[Database Schema]] | `@present`, `FreeSchema`, `FinCat` |
| [[C-Set]] / [[Functor]] $\mathcal{C} \to \mathbf{Set}$ | `@acset_type`, `@acset`, `FinDomFunctor` |
| [[Natural Transformation]] / [[Graph Homomorphism]] | `ACSetTransformation`, `homomorphism(s)` |
| [[Limit]], [[Colimit]], [[Pushout]], [[Pullback]] | `limit`, `colimit`, `pushout`, `pullback`, `coequalizer` |
| [[Data Migration Functor|Data migration]] $\Delta_F, \Sigma_F, \Pi_F$ | `DeltaMigration`, `SigmaMigration`, `migrate` |
| [[Cospan]], [[Decorated Cospan]], [[Structured Cospan]] | `Cospan`, `StructuredCospan`, `OpenCSet` |
| [[Hypergraph Category]] / [[Wiring Diagram|UWDs]] | `@relation`, `oapply`, `UndirectedWiringDiagram` |
| [[Prop]] / [[Signal Flow Graph]] | `ThBiproductCategory`, `@theory`, `Catlab.Programs` |
| [[Operad]] | `oapply` (operad algebras), `Catlab.WiringDiagrams` |
| [[Subobject]] lattice | `Subobject`, `meet`, `join`, `top`, `bottom` |
