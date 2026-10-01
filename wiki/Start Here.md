#index #annotation

Welcome. This vault is a wiki built from four sources — Fong & Spivak's *An Invitation to Applied Category Theory: Seven Sketches in Compositionality* ("7 Sketches"), David Spivak's *Category Theory for Scientists* ([arXiv:1302.6946](https://arxiv.org/abs/1302.6946), "CTfS"), Bartosz Milewski's *The Dao of Functional Programming* ("DaoFP"), and Owen Lynch's *Kittenlab.jl* lectures — with every concept in its own note. This page explains how the vault is organized and proposes an order in which to read it. For the complete list of notes grouped by topic see the [[Map of Content]].

## How the vault is organized

- **Concept notes** (root folder): one note per definition, theorem or construction, e.g. [[Category]], [[Adjunction]], [[Monad]]. Each starts with a tag line (`#definition #theorem #proof #example #program #annotation`), gives the definition, then bullets that merge what each book contributes, a `> Sources:` line with exact section/exercise numbers, and a code block with tabs for **Julia** ([[Catlab]] v0.16), **Lean 4** (Mathlib names, given as `#check`s) and **Haskell**. The Julia tab opens with a **Docs:** line linking the Catlab/GATlab/ACSets documentation (or Kittenlab lecture) for the functions it uses, and — when it reuses types defined in another note — a **Builds on:** line naming that note.
- **Exercises/** — one note per chapter (`7S Chapter 3 Exercises`, `DaoFP Chapter 9 Exercises`, `CTfS Chapter 4 Exercises`), with a heading per exercise; links from concept notes point at these headings, e.g. [[7S Chapter 6 Exercises#Exercise 6.10|7S Exercise 6.10]].
- **Solutions/** — the matching `… Chapter N Solutions` notes: worked solutions (7 Sketches' come from its Appendix A; DaoFP's and CTfS's — the latter has no printed solutions — are written out for this wiki), often with code. The CTfS files hold the exercises referenced from concept notes (about a hundred of CTfS's ~260), numbered as in the book, e.g. [[CTfS Chapter 4 Exercises#Exercise 4.4.1.7|CTfS Exercise 4.4.1.7]]. Each exercise links to its solution and back.
- Links to notes that do not exist are intentional loose ends; the graph view will show them.
- Formulas are LaTeX (`$…$` inline, `$$…$$` display); commutative diagrams are `tikz` blocks (tikz-cd).

## How to read a note

Start with the definition paragraph, then the *Sources* line tells you where to read more in the books. The bullets are where the sources are reconciled — e.g. [[Product]] gives 7 Sketches' universal-property definition, CTfS's grid of pairs and olog readings, DaoFP's "mapping-in" programmer's view, and Kittenlab's typed products in one place. CTfS contributes most of the *worked examples from science and everyday life* (ologs, databases, state machines, Markov chains, sheaves of measurements); look for sections titled "Examples from Category Theory for Scientists". The code tabs are meant to be run: Julia snippets were executed against Catlab 0.16 (see [[Catlab]] for setup); Lean lines point at Mathlib declarations; Haskell shows the idiom DaoFP uses.

## Suggested order of study

Each stage lists the notes to read (roughly in order) and the exercises that go with them. Stages 1–4 are the shared core; after that the tracks split by interest.

### Stage 0 — Sets and ologs as a warm-up (CTfS Ch. 2)
For readers who want the foundations spelled out with pictures and everyday examples before anything categorical.
[[Olog]] → [[Set]], [[Function]], [[Function Composition]], [[Isomorphism]], [[Cardinality]] → [[Product]], [[Coproduct]], [[Pullback]], [[Equalizer]], [[Finite Limits in Set]] → [[Equivalence Relation]], [[Quotient Set]], [[Pushout]], [[Coequalizer]], [[Finite Colimits in Set]] → [[Section and Retraction]], [[Arithmetic of Sets]], [[Currying]], [[Power Set]], [[Subobject Classifier]], [[Simplicial Complex]] → [[Multiset]], [[Indexed Set]], [[Slice Category]].
*Exercises*: [[CTfS Chapter 2 Exercises]].

### Stage 1 — Sets, preorders and the first adjunctions (7 Sketches Ch. 1)
The gentlest entry: order theory as category theory in miniature.
[[Set]], [[Function]], [[Relation]], [[Partition]], [[Equivalence Relation]] → [[Preorder]], [[Hasse Diagram]], [[Partial Order]], [[Monotone Map]] → [[Meet]], [[Join]], [[Upper Set]] → [[Generative Effect]] → [[Galois Connection]], [[Right Adjoints Preserve Meets]], [[Adjoint Functor Theorem for Preorders]] → [[Closure Operator]], [[Pushforward and Pullback of Partitions]].
*Exercises*: [[7S Chapter 1 Exercises]]; CTfS §3.4 in [[CTfS Chapter 3 Exercises]].

### Stage 2 — Categories, functors, natural transformations (7 Sketches Ch. 3, DaoFP Ch. 1–3, 8–9, Kittenlab 1–7)
[[Category]] (read this first — it is the template for the whole vault), [[Free Category]], [[Presentation of a Category]], [[Isomorphism]], [[Monomorphism]], [[Epimorphism]] → [[Functor]], [[Natural Transformation]], [[Functor Category]] → [[Universal Property]], [[Terminal Object]], [[Initial Object]], [[Product]], [[Coproduct]] → [[Representable Functor]], [[Yoneda Lemma]], [[Yoneda Embedding]].
Kittenlab's angle: [[Category of Finite Sets]], [[Graph Homomorphism]], [[C-Set]], [[Database Schema]].
CTfS's angle ("categories without admitting it", then basic category theory): [[Monoid]], [[Free Monoid]], [[Presentation of a Monoid]], [[Monoid Action]], [[Finite State Machine]], [[Group Action]], [[Endomorphism Monoid]], [[Graph]], [[Discrete Dynamical System]] → [[Categories and Schemas are Equivalent]], [[Full and Faithful Functor]], [[Equivalence of Categories]], [[Skeleton]], [[Simplex Category]], [[Symmetric Graph]], [[Category of Elements]].
*Exercises*: [[7S Chapter 3 Exercises]]; [[CTfS Chapter 3 Exercises]], [[CTfS Chapter 4 Exercises]]; [[DaoFP Chapter 2 Exercises]], [[DaoFP Chapter 3 Exercises]], [[DaoFP Chapter 8 Exercises]], [[DaoFP Chapter 9 Exercises]].

### Stage 3 — Limits, colimits, adjunctions (7 Sketches Ch. 3 & 6, DaoFP Ch. 9–10, Kittenlab 8–13)
[[Cone]], [[Cone Category]], [[Limit]], [[Pullback]], [[Equalizer]], [[Finite Limits in Set]] → [[Cocone]], [[Colimit]], [[Pushout]], [[Coequalizer]], [[Finite Colimits in Set]], [[Colimits and Connection]] → [[Adjunction]], [[Unit and Counit of an Adjunction]], [[Free-Forgetful Adjunction]], [[Right Adjoints Preserve Limits]], [[Adjoint Functor Theorem]] → [[Data Migration Functor]] → [[Exponential Object]], [[Currying]], [[Cartesian Closed Category]].
*Exercises*: [[7S Chapter 3 Exercises#Exercise 3.79|7S 3.79]]–3.101 and [[7S Chapter 6 Exercises#Exercise 6.3|6.3]]–6.41; [[DaoFP Chapter 9 Exercises]], [[DaoFP Chapter 10 Exercises]]; CTfS §4.5 and §5.1 in [[CTfS Chapter 4 Exercises]], [[CTfS Chapter 5 Exercises]].

### Stage 4 — Monoidal structure (7 Sketches Ch. 2 & 4)
[[Symmetric Monoidal Preorder]], [[Wiring Diagram]], [[Resource Theory]] → [[Enriched Category]], [[Lawvere Metric Space]], [[Quantale]], [[Matrix Multiplication in a Quantale]] → [[Monoidal Category]], [[Symmetric Monoidal Category]], [[Monoid Object]], [[Monoidal Functor]] → [[Profunctor]], [[Feasibility Relation]], [[Compact Closed Category]].
*Exercises*: [[7S Chapter 2 Exercises]], [[7S Chapter 4 Exercises]].

After stage 4, pick a track (or interleave them).

### Track A — Applied category theory: props, circuits, toposes (7 Sketches Ch. 5–7)
[[Prop]], [[Presentation of a Prop]], [[Signal Flow Graph]], [[Prop of Matrices]], [[Graphical Linear Algebra]] → [[Frobenius Monoid]], [[Hypergraph Category]], [[Decorated Cospan]], [[Structured Cospan]], [[Operad]], [[Undirected Wiring Diagram]], [[Petri Net]] → [[Topos]], [[Subobject Classifier]], [[Internal Logic of a Topos]], [[Heyting Algebra]], [[Topological Space]], [[Sheaf]], [[Sheaf of Sections]], [[Quantification]], [[Modality]], [[Topos of Behavior Types]], [[Temporal Logic]].
*Exercises*: [[7S Chapter 5 Exercises]], [[7S Chapter 6 Exercises#Exercise 6.48|7S 6.48]]–6.96, [[7S Chapter 7 Exercises]].

### Track B — Programming with categories: types, recursion, monads (DaoFP Ch. 4–7, 12–16)
[[Sum Type]], [[Cartesian Category]], [[Bicartesian Closed Category]] → [[Natural Numbers Object]], [[List]] → [[Algebra of an Endofunctor]], [[Initial Algebra]], [[Coalgebra of an Endofunctor]], [[Terminal Coalgebra]] → [[Side Effects as Functors]], [[Monad]], [[Kleisli Category]], [[Kleisli Instance]], [[Maybe Monad]], [[Power Set Monad]], [[Distribution Monad]], [[Markov Chain]], [[State Monad]], [[List Monad]], [[Continuation Monad]], [[Do Notation]] → [[Free Monad]], [[Applicative Functor]], [[Functorial Strength]] → [[String Diagram]], [[Monads from Adjunctions]], [[Monad Transformer]], [[Eilenberg-Moore Category]] → [[Comonad]], [[Store Comonad]], [[Lens]].
*Exercises*: [[DaoFP Chapter 4 Exercises]]–[[DaoFP Chapter 7 Exercises|7]], [[DaoFP Chapter 12 Exercises]]–[[DaoFP Chapter 16 Exercises|16]].

### Track C — Dependent types (DaoFP Ch. 11)
[[Dependent Type]], [[Fiber]], [[Slice Category]], [[Base Change Functor]], [[Dependent Sum]], [[Dependent Product]], [[Locally Cartesian Closed Category]], [[Equality Type]]. Pairs well with the [[Sheaf of Sections]] note from Track A.
*Exercises*: [[DaoFP Chapter 11 Exercises]].

### Track D — Advanced: (co)ends, optics, Kan extensions, enrichment (DaoFP Ch. 17–20, 7 Sketches Ch. 4.3)
[[Coend]], [[End]], [[Ninja Yoneda Lemma]], [[Day Convolution]], [[Bicategory of Profunctors]], [[Existential Lens]] → [[Tannakian Reconstruction]], [[Tambara Module]], [[Profunctor Optics]] → [[Kan Extension]], [[Codensity Monad]] → [[Enriched Functor]], [[Enriched Natural Transformation]], [[Weighted Limit]].
*Exercises*: [[DaoFP Chapter 17 Exercises]]–[[DaoFP Chapter 20 Exercises|20]].

### Track E — Category theory for machine learning (arXiv papers; the road to Lenticulum.jl)
The research literature on categorical deep learning, categorical probability and compositional inference. Every note cites numbered definitions from the papers, which have their own notes in [[Papers]]. Prerequisites: Stages 2–4 ([[Monoidal Category]], [[String Diagram]], [[Lens]], [[Kleisli Category]], [[Frobenius Monoid]], [[Hypergraph Category]]).
1. *Two-dimensional structure*: [[Bicategory]], [[Lax Functor]], [[Grothendieck Construction]], [[Actegory]].
2. *Learning with lenses*: [[Para Construction]] → [[Optic]] → [[Parametric Lens]] → [[Cartesian Differential Category]] → [[Reverse Derivative Category]] → [[Gradient-Based Learning with Parametric Lenses]], [[Backprop as Functor]].
3. *Categorical probability*: [[Copy-Discard Category]] → [[Markov Category]] → [[Giry Monad]] → [[Almost-Sure Equality]] → [[Conditionals and Disintegration]] → [[Bayesian Inversion]] → [[Conditional Independence]].
4. *Compositional inference*: [[Open Model]] → [[Bayesian Lens]] → [[Variational Free Energy]] → [[Statistical Game]]; and games proper, [[Open Game]].
5. *Acausal models and bookkeeping*: [[Hypergraph Category]] (factor graphs) → [[Partial Markov Category]] → [[Gaussian Relations]] → [[Rig]] (semirings in message passing) → [[Graded Monad]] → [[Double Category]].

This track is the categorical background of [Lenticulum.jl](https://mathstruct.org/Lenticulum.jl/dev/vault/), whose theory vault links back to these notes.

### Track F — Category theory for compilers and databases (arXiv papers; the road to Sophia)
The categorical side of programming-language semantics, rewriting and databases: what a program *is*, when two programs are *the same*, and how a compiler and a query engine can be the same machine. Every note cites numbered definitions from the papers in [[Papers]]. Prerequisites: Stages 2–4 and Tracks B–C ([[Initial Algebra]], [[Monad]], [[Kleisli Category]], [[C-Set]], [[Data Migration Functor]], [[Dependent Type]], [[Locally Cartesian Closed Category]]).
1. *Syntax and identity*: [[Polynomial Functor]] → [[Abstract Syntax with Binding]] → [[Congruence]] → [[Bisimulation]].
2. *Semantics*: [[Curry-Howard-Lambek Correspondence]] → [[Lawvere Theory]] → [[Category with Families]]; effects: [[Algebraic Effects and Handlers]] → [[Freyd Category]] → [[Call-by-Push-Value]]; resources: [[Linear-Non-Linear Adjunction]].
3. *Equivalence and translation*: [[Contextual Equivalence]] → [[Logical Relations]] → [[Compiler Correctness]] → [[Institution]].
4. *Rewriting*: [[E-Graph]] (equality saturation) and [[Double-Pushout Rewriting]] (graph rewriting).
5. *Databases and queries*: [[Attributed C-Set]] → [[Algebraic Database]] → [[Conjunctive Query]] → [[Cartesian Bicategory]] → [[Least Fixed Point]] (Datalog) → [[Provenance Semiring]] → [[Change Action]] (incremental computation); and [[Relational Lens]], [[Monad Comprehension]].

This track is the categorical background of [Sophia](https://mathstruct.org/Sophia/), a content-addressed graph database of code whose design notes link back to these notes.

## Three ways in, depending on who you are

- **Coming from programming (Haskell/Julia)**: read [[Category]], [[Functor]], [[Natural Transformation]], then jump to Track B and use the Haskell tabs; come back to Stage 3 when you meet [[Adjunction]] in [[Monads from Adjunctions]].
- **Coming from mathematics**: Stages 1–4 in order, then Track A; the Lean tabs give the Mathlib names to formalize what you read.
- **Coming from the sciences** (CTfS's intended reader): Stage 0, then [[Database Schema]], [[C-Set]], [[Data Migration Functor]], [[Sheaf]] and [[Markov Chain]] — the examples are ologs, experiments and databases rather than proofs.
- **Coming from machine learning research**: Stage 2 quickly, then Track E; the [[Papers]] index lists what to read in the original.
- **Coming from compilers, programming languages or databases**: Stage 2, Track B, then Track F; the Julia tabs there run against Catlab and AlgebraicRewriting.jl, the Lean tabs against current Mathlib.
- **Wanting to compute**: install Catlab 0.16 as described in [[Catlab]] and work through the Julia tabs of [[C-Set]], [[Data Migration Functor]], [[Colimit]], [[Undirected Wiring Diagram]] and [[Decorated Cospan]] — Kittenlab's material is concentrated there.

## Cross-cutting themes to watch for

- The same idea at three levels: preorder → category → enriched/2-category ([[Galois Connection]] → [[Adjunction]]; [[Closure Operator]] → [[Monad]]; [[Yoneda Lemma for Preorders]] → [[Yoneda Lemma]] → [[Ninja Yoneda Lemma]]; [[Monoidal Closed Preorder]] → [[Monoidal Closed Category]]).
- "Composition is something that happens between things": [[Wiring Diagram]]s, [[String Diagram]]s and [[Undirected Wiring Diagram]]s are the same graphical language seen from 7 Sketches' and DaoFP's sides.
- Universal properties everywhere: [[Universal Property]] collects the pattern that [[Kan Extension]] finally subsumes.
