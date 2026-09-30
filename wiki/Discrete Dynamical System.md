#example #definition

The schema $\mathsf{DDS}$ has one object $\mathsf{State}$ and one arrow $\mathsf{next} : \mathsf{State} \to \mathsf{State}$ (no equations). A $\mathsf{DDS}$-instance $I : \mathsf{DDS} \to \mathbf{Set}$ is a set of states with a function `next`: a **discrete dynamical system** — a deterministic machine which, in each state, moves to a uniquely determined next state.

> Sources: 7 Sketches §3.4.1 (Eq. 3.65–3.66), Exercise 3.67; Example 3.46 (idempotent variant); DaoFP Chapter 13 (coalgebras $a \to F a$ as state machines); CTfS Example 3.5.2.9, Application 3.5.2.10, Exercises 3.5.2.12–3.5.2.13, 4.5.1.7, 4.5.1.22, Example 4.2.3.3, Example 5.3.4.3

**Eq. (3.65).** States $1..7$ with $\mathsf{next} = (4, 4, 5, 5, 5, 7, 6)$: states $1, 2 \to 4 \to 5 \circlearrowleft$, $3 \to 5$, and the 2-cycle $6 \leftrightarrow 7$.

**CTfS's example** (Example 3.5.2.9): states $A, \dots, H$ with $\mathsf{next}$ = A↦B, B↦C, C↦C, D↦B, E↦C, F↦G, G↦H, H↦G — two "basins": everything in $\{A,\dots,E\}$ flows to the fixed point $C$, and $F$ falls into the 2-cycle $G \leftrightarrow H$. This particular instance satisfies the path equation $\mathsf{next}^4 = \mathsf{next}^2$, which cuts the infinitely many paths of the schema down to four classes $\mathrm{id}, \mathsf{next}, \mathsf{next}^2, \mathsf{next}^3$ ([[CTfS Chapter 3 Exercises#Exercise 3.5.2.12|CTfS Exercise 3.5.2.12]]). More interpretations: a "quantum-time universe" with one row per state of the universe (Application 3.5.2.10); a chess program choosing a move in every position, where the game-ending positions are fixed points ([[CTfS Chapter 3 Exercises#Exercise 3.5.2.13|CTfS Exercise 3.5.2.13]]); a management hierarchy $\mathsf{mgr}$ with $\mathsf{mgr}^8 = \mathsf{mgr}^7$.

**Products and sums** (CTfS Exercises 4.5.1.7, 4.5.1.22): the product of two DDSs runs both systems in lockstep on pairs of states, $\mathsf{next}(i, j) = (\mathsf{next}\,i, \mathsf{next}\,j)$; the coproduct runs them side by side. **Variants**: replacing $\mathbb{N}$ by the topological monoid $(\mathbb{R}, 0, +)$ and $\mathbf{Set}$ by $\mathbf{Top}$ gives a *continuous* dynamical system (a flow, CTfS Example 4.2.3.3); letting $\mathsf{next}$ return a probability distribution gives a [[Markov Chain]] (a Kleisli instance for the [[Distribution Monad]]).

**Migration to a graph.** The functor $F : \mathsf{Gr} \to \mathsf{DDS}$ sending both $\mathsf{Arrow}, \mathsf{Vertex} \mapsto \mathsf{State}$, $\mathsf{source} \mapsto \mathrm{id}$, $\mathsf{target} \mapsto \mathsf{next}$ gives the [[Data Migration Functor|pullback]] $\Delta_F(I) = F \mathbin{;} I$: a graph with one vertex and one arrow per state, arrow $s$ going from $s$ to $\mathsf{next}(s)$ — "what I do next is determined by what I am now". With $G$ swapping the roles of source and target, the arrows point backwards ([[7S Chapter 3 Exercises#Exercise 3.67|7S Exercise 3.67]]). Conversely $\Sigma_F, \Pi_F$ turn any graph into a DDS.

Categorically, a DDS is an [[Algebra of an Endofunctor|algebra]] and [[Coalgebra of an Endofunctor|coalgebra]] of the identity functor, a [[Monoid|$\mathbb{N}$-set]] (a functor from the one-object category $\mathbb{N}$ of [[Free Category|Example 3.13]] to $\mathbf{Set}$), and a $\mathbf{Set}$-valued [[Representable Functor|non-representable]] functor in general.

````tabs
tab: Julia
**Docs:** [FinCats](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FinCats) · [Data migration](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.FunctorialDataMigrations) · [ACSets API](https://algebraicjulia.github.io/ACSets.jl/stable/api/) · [Graphs](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/graphs/) · [Theories & presentations](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/theories/)
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
