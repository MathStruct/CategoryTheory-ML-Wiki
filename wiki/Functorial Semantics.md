#definition #theorem #annotation

**Functorial semantics** (Lawvere) separates a language into **syntax** — a structure such as a [[Prop]], [[Presentation of a Category|presented category]] or algebraic theory in which expressions are built compositionally — and **semantics** — a [[Functor]] from the syntax to a structure of meanings that has the same compositional grammar. 7 Sketches' running example: [[Signal Flow Graph|signal flow graphs]] form the prop $\mathbf{SFG}_R$ (syntax: series and parallel composition), matrices form the prop $\mathbf{Mat}(R)$, and interpretation is the prop functor $S : \mathbf{SFG}_R \to \mathbf{Mat}(R)$ (Theorem 5.53), so "matrices give functorial semantics for signal flow diagrams".

> Sources: 7 Sketches §5.3.5 ("The idea of functorial semantics"), §5.5 ("Perhaps the most significant idea in this chapter"), Remark 5.74, §6.4 (decorated cospans), §6.5 (operad algebras), §7.4.6 (type theories and semantics); Lawvere's thesis [Law04]; Kittenlab Lecture 15 ("syntax and semantics are dual").

**Why it matters — compositionality.** The meaning $S(g)$ of a big graph is computed by (1) splitting $g$ into little pieces, (2) computing the simple matrix of each piece, (3) reassembling with matrix multiplication and direct sum. For large graphs "composing matrices is much faster than tracing paths".

**Other instances.**
- A [[Monoid Object]] in $\mathcal{C}$ is a strict monoidal functor from the prop presented by the theory of monoids (Remark 5.74); models of any Lawvere/algebraic theory are product-preserving functors.
- [[C-Set|Database instances]] are functors $\mathcal{C} \to \mathbf{Set}$ from a schema (Chapter 3); [[Data Migration Functor|migration]] is precomposition.
- [[Decorated Cospan|Decorated cospans]] $F : (\mathbf{FinSet}, \sqcup) \to (\mathbf{Set}, \times)$ and [[Operad|operad algebras]] $\mathbf{Cospan} \to \mathbf{Set}$ give semantics to [[Hypergraph Category|circuit diagrams]] (Chapter 6).
- The [[Cartesian Closed Category|simply typed lambda calculus]] / [[Topos|type theories]] have semantics in CCCs / toposes (Chapter 7).
- [[Prop of Matrices|$\mathbf{Mat}(R)$]] itself has semantics $U : \mathbf{Mat}(R) \to \mathbf{Set}$, $n \mapsto R^n$ ([[7S Exercise 5.69]]); behaviours $B : \mathbf{SFG}^+_R \to \mathbf{Rel}_R$ interpret feedback ([[Graphical Linear Algebra]]).

The [[Free Prop|universal property of free]] and [[Presentation of a Prop|presented]] structures is what makes defining such functors easy: specify the images of the generators and check the equations.
