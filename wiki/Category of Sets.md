#definition #example

The **category of sets**, $\mathbf{Set}$:

(i) $\mathrm{Ob}(\mathbf{Set})$ is the collection of all [[Set|sets]];
(ii) $\mathbf{Set}(S, T) = \{f : S \to T \mid f \text{ is a function}\}$;
(iii) the identity is $\mathrm{id}_S(s) = s$;
(iv) composition is $(f \mathbin{;} g)(s) = g(f(s))$.

Unitality and associativity hold, so $\mathbf{Set}$ is a category — "the most important category in mathematics".

> Sources: 7 Sketches Definition 3.24, Remark 3.26, Example 3.29, Exercise 3.25, §3.5.3, §7.2; Kittenlab Lecture 3 ("sets" as predicates; $\mathsf{Jul}$, the category of Julia types); DaoFP §8.1 ("Category of sets"), §10.11; CTfS Chapter 2 ("The category of sets": all of it is "an investigation of our first category"), Example 4.1.1.3, §5.2.1 (the dictionary between $\mathbf{Set}$ and $\mathcal{C}$-$\mathbf{Set}$)

## Properties and role

- [[Isomorphism|Isomorphisms]] are [[Bijection|bijections]]; [[Monomorphism|monos]] are [[Injection|injections]], [[Epimorphism|epis]] are [[Surjection|surjections]]. $|\mathbf{Set}(\underline{2}, \underline{3})| = 9$ ([[7S Chapter 3 Exercises#Exercise 3.25|7S Exercise 3.25]]); $|\mathbf{Set}(B, C)| = |C|^{|B|}$ — the [[Exponential Object]] $C^B$.
- The [[Terminal Object]] is any singleton $\{\bullet\}$; the [[Initial Object]] is $\varnothing$; [[Product|products]] are cartesian products, [[Coproduct|coproducts]] disjoint unions; all finite [[Limit|limits]] are computed by the tuple formula of [[Finite Limits in Set]], and all [[Colimit|colimits]] exist too: $\mathbf{Set}$ is complete and cocomplete (DaoFP §9.5). It is [[Cartesian Closed Category|cartesian closed]] and a [[Topos]] (7 Sketches Chapter 7: "$\mathbf{Set}$ as an exemplar topos").
- Categories are exactly $\mathbf{Set}$-[[Enriched Category|enriched categories]] (Remark 3.26): the hom-sets live in $\mathbf{Set}$, so "the study of $\mathbf{Set}$ yields insights into categories". Functors $\mathcal{C} \to \mathbf{Set}$ are [[C-Set|C-sets]] / [[Database Schema|database instances]] / co-presheaves; [[Representable Functor|representables]] and the [[Yoneda Lemma]] live here.
- Sets are [[Discrete Category|discrete categories]] (DaoFP: "a set is a category with no structure"), and databases on the schema $\underline{1}$ (7 Sketches §3.3.1: "sets are databases whose schema consists of a single vertex", one-column tables / controlled vocabularies).
- **$\mathbf{Set}$ as a template** (CTfS §5.2.1). For any schema/category $\mathcal{C}$ the category of instances $\mathcal{C}\text{-}\mathbf{Set}$ is a [[Topos]], so "just about every consideration we made for sets holds for instances on any schema":

  | in $\mathbf{Set}$ | in $\mathcal{C}\text{-}\mathbf{Set}$ |
  |---|---|
  | set, function | instance, natural transformation |
  | element | [[Representable Functor\|representable functor]] |
  | empty set | [[Initial Object\|initial object]] |
  | $\mathbb N$ | [[Natural Numbers Object\|natural numbers object]] |
  | (co)limits, exponentials, "familiar" arithmetic | the same, computed pointwise ([[Arithmetic of Sets]]) |
  | power set $2^X$, characteristic functions | power object $\Omega^X$, [[Subobject Classifier\|characteristic maps]] |
  | surjections, injections | [[Epimorphism\|epimorphisms]], [[Monomorphism\|monomorphisms]] |
- $\mathbf{Set}$ is not small (there is no set of all sets) but *locally small* (all hom-sets are sets); Kittenlab: there is a set of all *computable* sets. DaoFP: programming is modeled "to the lowest approximation" in $\mathbf{Set}$ — but there are more set functions than algorithms, and some algorithms diverge.

````tabs
tab: Julia
**Docs:** [Sets](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.Sets) — Kittenlab [Lecture 3](https://algebraicjulia.github.io/Kittenlab.jl/lecture3.html)

**Builds on:** [[Set]] (`ComputableSet`) — run that note's Julia code first.
```julia
# Kittenlab Lecture 3: a set is a predicate `Any -> Bool`; morphisms are Julia callables
struct TypeSet <: ComputableSet; T::Type end
Base.in(x, χ::TypeSet) = x isa χ.T
# A → B is the set of callables f with f(a) ∈ B for all a ∈ A (not checkable for infinite A)
```
Catlab version (run in a fresh Julia session — Catlab exports its own `compose`, `id`, `FinFunction`, …):
```julia
# Catlab: SetOb / SetFunction model Set (types as sets), FinSet models FinSet
using Catlab
X = TypeSet(Int)                        # the set of Ints as an object of Set
f = SetFunction(x -> x + 1, X, X)
compose(f, f)(1)                        # 3
```
tab: Lean
```lean
open CategoryTheory
#check (Type u)                               -- the category of types (sets in universe u)
example : Category (Type u) := inferInstance  -- types.instCategory: Hom X Y := X → Y
#check @CategoryTheory.mono_iff_injective
#check @CategoryTheory.epi_iff_surjective
#check @CategoryTheory.types.terminal          -- PUnit; initial: PEmpty
```
tab: Haskell
```haskell
-- Hask: objects are types, morphisms are functions; composition is (.), identity is id
-- (Hask is only approximately Set: laziness, bottom, and non-total functions differ.)
newtype Hask a b = Hask (a -> b)
```
````
