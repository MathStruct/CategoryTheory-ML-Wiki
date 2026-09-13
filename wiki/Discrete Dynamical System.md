#example #definition

The schema $\mathsf{DDS}$ has one object $\mathsf{State}$ and one arrow $\mathsf{next} : \mathsf{State} \to \mathsf{State}$ (no equations). A $\mathsf{DDS}$-instance $I : \mathsf{DDS} \to \mathbf{Set}$ is a set of states with a function `next`: a **discrete dynamical system** — a deterministic machine which, in each state, moves to a uniquely determined next state.

> Sources: 7 Sketches §3.4.1 (Eq. 3.65–3.66), Exercise 3.67; Example 3.46 (idempotent variant); DaoFP Chapter 13 (coalgebras $a \to F a$ as state machines).

**Eq. (3.65).** States $1..7$ with $\mathsf{next} = (4, 4, 5, 5, 5, 7, 6)$: states $1, 2 \to 4 \to 5 \circlearrowleft$, $3 \to 5$, and the 2-cycle $6 \leftrightarrow 7$.

**Migration to a graph.** The functor $F : \mathsf{Gr} \to \mathsf{DDS}$ sending both $\mathsf{Arrow}, \mathsf{Vertex} \mapsto \mathsf{State}$, $\mathsf{source} \mapsto \mathrm{id}$, $\mathsf{target} \mapsto \mathsf{next}$ gives the [[Data Migration Functor|pullback]] $\Delta_F(I) = F \mathbin{;} I$: a graph with one vertex and one arrow per state, arrow $s$ going from $s$ to $\mathsf{next}(s)$ — "what I do next is determined by what I am now". With $G$ swapping the roles of source and target, the arrows point backwards ([[7S Chapter 3 Exercises#Exercise 3.67|7S Exercise 3.67]]). Conversely $\Sigma_F, \Pi_F$ turn any graph into a DDS.

Categorically, a DDS is an [[Algebra of an Endofunctor|algebra]] and [[Coalgebra of an Endofunctor|coalgebra]] of the identity functor, a [[Monoid|$\mathbb{N}$-set]] (a functor from the one-object category $\mathbb{N}$ of [[Free Category|Example 3.13]] to $\mathbf{Set}$), and a $\mathbf{Set}$-valued [[Representable Functor|non-representable]] functor in general.

````tabs
tab: Julia
```julia
using Catlab
@present SchDDS(FreeSchema) begin State::Ob; next::Hom(State, State) end
@acset_type DDS(SchDDS)
I = @acset DDS begin State = 7; next = [4, 4, 5, 5, 5, 7, 6] end

# pull back along F : Gr → DDS to get a graph
F = FinFunctor(Dict(:V => :State, :E => :State), Dict(:src => id(SchDDS[:State]), :tgt => :next),
               FinCat(SchGraph), FinCat(SchDDS))
G = migrate(Graph, I, DeltaMigration(F))   # Δ_F(I): the graph of Eq. (3.66)
src(G), tgt(G)                    # ([1..7], [4,4,5,5,5,7,6])
```
tab: Haskell
```haskell
data DDS s = DDS { states :: [s], next :: s -> s }
-- Δ_F: the graph with an arrow s -> next s for each state
toGraph :: DDS s -> Graph s s
toGraph (DDS ss n) = Graph ss ss id n
```
````
