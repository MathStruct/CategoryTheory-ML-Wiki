#definition #example #theorem

Let $F : \mathcal{C} \to \mathcal{D}$ be a [[Functor]] between [[Database Schema|schemas]]. Three **data migration functors** relate [[C-Set|instances]]:

- **$\Delta_F : \mathcal{D}\text{-}\mathbf{Inst} \to \mathcal{C}\text{-}\mathbf{Inst}$** (pullback / "duplicate or destroy"): $\Delta_F(I) := F \mathbin{;} I$, precomposition; on morphisms $(\alpha_F)_c := \alpha_{F(c)}$ (Definition 3.68). It duplicates or destroys tables and columns.
- **$\Sigma_F : \mathcal{C}\text{-}\mathbf{Inst} \to \mathcal{D}\text{-}\mathbf{Inst}$**, the *left adjoint* of $\Delta_F$ ("sum": union data), built from [[Colimit|colimits]] in $\mathbf{Set}$.
- **$\Pi_F : \mathcal{C}\text{-}\mathbf{Inst} \to \mathcal{D}\text{-}\mathbf{Inst}$**, the *right adjoint* ("product": pair/query data — database programmers say *join*), built from [[Limit|limits]] in $\mathbf{Set}$.
$$\Sigma_F \dashv \Delta_F \dashv \Pi_F.$$

> Sources: 7 Sketches §3.4 (Definition 3.68, §3.4.3–3.4.4, Eq. 3.77, Exercises 3.67, 3.76, 3.78), Remark 3.100, §3.6 ("All concepts are Kan extensions"); FQL; DaoFP Chapter 19 ($\Sigma_F, \Pi_F$ are the left and right [[Kan Extension|Kan extensions]] along $F$), Chapter 11 ([[Dependent Sum]]/[[Dependent Product]] along a map of types); 7 Sketches §1.4.2 (the preorder shadow: [[Pushforward and Pullback of Partitions]]).

## Examples

- $\mathsf{Gr} \to \mathsf{DDS}$: $\Delta_F$ turns a [[Discrete Dynamical System]] into its graph (§3.4.1).
- Airline seats (Eq. 3.5): $F : \mathcal{A} \to \mathcal{B}$ sends $\mathsf{Economy}, \mathsf{FirstClass} \mapsto \mathsf{AirlineSeat}$. $\Delta_F$ copies the seat table into both; $\Sigma_F(I)(\mathsf{AirlineSeat}) = I(\mathsf{Economy}) \sqcup I(\mathsf{FirstClass})$; $\Pi_F(I)(\mathsf{AirlineSeat})$ is the set of pairs $(e, f)$ with the same price and position — presumably empty here, but with "Rewards Program" and "First Class Seats" it finds the first-class seats in the rewards program: a **query**.
- **Single-set summaries** ($\mathcal{D} = \underline{\mathbf{1}}$, $! : \mathcal{C} \to \underline{\mathbf{1}}$, [[7S Exercise 3.76]]): identifying $\underline{1}\text{-}\mathbf{Inst} \simeq \mathbf{Set}$, $\Sigma_!(I) = \mathrm{colim}\,I$ and $\Pi_!(I) = \lim I$. For the email schema $\mathsf{Email} \rightrightarrows \mathsf{Address}$ (Eq. 3.77, $\cong \mathsf{Gr}$, [[7S Exercise 3.78]]), $\Sigma_!(I)$ is the set of emailing groups (connected components: Bob–Grace–Pat–Emmy, Sue–Doug) — a typical $\Sigma$ *quotient*; $\Pi_!(I)$ is the set of self-to-self emails ($\mathsf{Em\_6}$) — a typical $\Pi$ *selection*. See [[Finite Limits in Set]].

"Everything follows from the definition of adjoint functors"; complex migrations are built from $\Delta, \Sigma, \Pi$ — "in practice essentially all useful migrations". The word *pullback* here is not the [[Pullback|limit]] of a cospan, though via the [[Category of Elements]] and discrete opfibrations it is a pullback in $\mathbf{Cat}$ (Remark 3.100).

````tabs
tab: Julia
```julia
using Catlab
@present SchDDS(FreeSchema) begin State::Ob; next::Hom(State, State) end
@acset_type DDS(SchDDS)
I = @acset DDS begin State = 7; next = [4, 4, 5, 5, 5, 7, 6] end
F = FinFunctor(Dict(:V => :State, :E => :State), Dict(:src => id(SchDDS[:State]), :tgt => :next),
               FinCat(SchGraph), FinCat(SchDDS))

Δ = DeltaMigration(F)                    # pullback along F
G = migrate(Graph, I, Δ)                 # a Graph
ΣF = SigmaMigrationFunctor(F, Graph, DDS)   # left adjoint (computed by colimits)
J = ΣF(G)                                   # a DDS again (Σ_F Δ_F I → I is the counit; J ≇ I in general)
# Π-migrations (limits/queries) are expressed with conjunctive queries (`@migration` with `@join`)
```
tab: Lean
```lean
-- Δ_F is precomposition; Σ_F and Π_F are left/right Kan extensions along F
#check CategoryTheory.Functor.lan     -- left Kan extension functor (Σ_F)
#check CategoryTheory.Functor.ran     -- right Kan extension functor (Π_F)
#check CategoryTheory.Functor.lanAdjunction   -- lan ⊣ precomposition
```
tab: Haskell
```haskell
-- Δ_F on hand-rolled instances: precompose the object/arrow assignment
-- Σ_F / Π_F are Kan extensions: see the `Lan` and `Ran` types in Data.Functor.Kan (kan-extensions)
-- newtype Lan g h a = forall b. Lan (g b -> a) (h b)      -- Σ (a coend)
-- newtype Ran g h a = Ran (forall b. (a -> g b) -> h b)   -- Π (an end)
```
````
