#definition #example #program

A **Petri net** is a [[C-Set]] on the schema $\mathsf{Petri}$ presented by the graph with objects $S$ (species/places), $T$ (transitions), $I$ (input arcs), $O$ (output arcs) and morphisms $\mathrm{is} : I \to S$, $\mathrm{it} : I \to T$, $\mathrm{os} : O \to S$, $\mathrm{ot} : O \to T$ (Kittenlab Lecture 6). That is: a bipartite multigraph whose arcs run from species to transitions (inputs) and from transitions to species (outputs). Examples: the **SIR** epidemic model (species S, I, R; transitions infection $S + I \to 2I$, recovery $I \to R$) and the **Lotka–Volterra** predator–prey model.

> Sources: Kittenlab Lecture 6 ("Petri nets" as acsets), Lecture 7 (natural transformations preserve arcs), Lecture 10 (the [[Representable Functor|representables]] $\mathrm{Hom}(S, -)$ = a single species, $\mathrm{Hom}(I, -)$ = one species, one transition, one input arc), Lecture 13 ("typed Petri nets" in a [[Slice Category]]); AlgebraicPetri.jl.

- A morphism of Petri nets is a [[Natural Transformation]]: four functions preserving the sources and targets of arcs ("the naturality condition just states that the sources and targets of arcs are preserved").
- [[Limit|Limits]] and [[Colimit|colimits]] are pointwise; **open Petri nets** are [[Decorated Cospan|decorated/structured cospans]] with feet in $\mathbf{FinSet}$ mapping to species, composed by [[Pushout]]; [[Undirected Wiring Diagram|UWDs]] with `oapply` glue several at once (AlgebraicPetri). **Typed Petri nets** are objects of $\mathsf{Petri}/P$ for a type net $P$.
- Semantics: a Petri net with rates generates ODEs (mass-action kinetics) or a continuous-time Markov chain — [[Functorial Semantics]] again; 7 Sketches §6.6 mentions Markov processes and chemistry as [[Hypergraph Category|hypergraph-categorical]] network languages.

````tabs
tab: Julia
```julia
using Catlab
@present SchPetri(FreeSchema) begin
  (S, T, I, O)::Ob
  is::Hom(I, S); it::Hom(I, T)
  os::Hom(O, S); ot::Hom(O, T)
end
@acset_type PetriNet(SchPetri, index=[:is, :it, :os, :ot])
# SIR: species S=1, I=2, R=3; transitions infection=1, recovery=2
sir = @acset PetriNet begin
  S = 3; T = 2
  I = 3; is = [1, 2, 2]; it = [1, 1, 2]        # S + I → infection, I → recovery
  O = 3; os = [2, 2, 3]; ot = [1, 1, 2]        # infection → 2I, recovery → R
end
# AlgebraicPetri.jl provides `PetriNet`, `LabelledPetriNet`, `OpenPetriNet`, `oapply` and ODE semantics
```
tab: Haskell
```haskell
data PetriNet = PetriNet
  { species :: Int, transitions :: Int
  , inputs  :: [(Int, Int)]    -- (species, transition)
  , outputs :: [(Int, Int)]    -- (species, transition)
  }
sir :: PetriNet
sir = PetriNet 3 2 [(1,1),(2,1),(2,2)] [(2,1),(2,1),(3,2)]
```
````
