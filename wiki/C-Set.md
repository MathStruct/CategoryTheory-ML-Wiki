#definition #example #program

A **$\mathcal{C}$-set** (7 Sketches: a **$\mathcal{C}$-instance**; Kittenlab: an **acset**, "attributed C-set", pronounced to rhyme with *hatchet*; also **copresheaf**) on a small category $\mathcal{C}$ is a [[Functor]] $I : \mathcal{C} \to \mathbf{Set}$. When $\mathcal{C}$ is a [[Database Schema|schema]] — a finitely presented category — it is a *database instance*: each object becomes a table (a set of rows/IDs) and each morphism a column (a function to another table's IDs); functoriality enforces the path equations ("business rules"). External attributes (white nodes such as `string`) are forced to specific sets — Catlab's *attributes*, hence the "a" in acset.

> Sources: 7 Sketches §3.3.1–3.3.3 (Definition 3.44, Examples 3.46, 3.53, 3.56, Exercises 3.45, 3.48), §3.3.5 (Definition 3.60: the category $\mathcal{C}\text{-}\mathbf{Inst} = \mathbf{Set}^{\mathcal{C}}$), Remark 3.20; Kittenlab Lecture 6 ("ACSets in Julia"), 7, 8, 10, 12; DaoFP §9.7 (co-presheaves), §20.2 (enriched co-presheaves). Warning (7 Sketches footnote 5): an "instance" is the state of the whole database at an instant, not a row (the OO usage).

## Examples

| schema $\mathcal{C}$ | $\mathcal{C}$-set | source |
|---|---|---|
| $\underline{\mathbf{1}}$ | a [[Set]] (one-column table, "controlled vocabulary") | [[7S Exercise 3.45]]; $\mathbf{Set}^{\underline{1}} \simeq \mathbf{Set}$ (Example 3.56) |
| $\underline{\mathbf{2}} = \bullet \to \bullet$ | a [[Function]] (two tables, e.g. Beatles $\to$ instruments) | §3.3.1 |
| $\mathsf{Gr}$: $E \rightrightarrows V$ | a [[Graph]]; morphisms are [[Graph Homomorphism|graph homomorphisms]] | §3.3.5, Kittenlab L6 |
| $\mathsf{DDS}$: one loop `next` | a [[Discrete Dynamical System]] | §3.4.1 |
| loop $s$ with $s \mathbin{;} s = s$ | a set $Z$ with an idempotent $S : Z \to Z$: citizens $\mapsto$ president, $n \mapsto 0$, expressions $\mapsto$ their value, $n \mapsto$ smallest prime factor | Example 3.46 |
| loop with $s \mathbin{;} s = \mathrm{id}$ | an involution ("do-si-do", mirror image of a photo) | [[7S Exercise 3.48]] |
| $a \xrightarrow{f} b \rightrightarrows c$ with $f\mathbin{;}g = f\mathbin{;}h$ | secret-Santa: people, gifts, giver, receiver, self-gifters | [[7S Exercise 3.48]] |
| $\mathsf{Petri}$: $I \rightrightarrows S, T \leftleftarrows O$ | a [[Petri Net]] (SIR, Lotka–Volterra) | Kittenlab L6, L10 |
| $\mathsf{DPG}$ | a directed [[Port Graph]] / [[Wiring Diagram]] | Kittenlab L6 |
| `mySchema` | the Employee/Department database | §3.1 |
| $\mathcal{C}(x, -)$ | the [[Representable Functor|representable]] $y_{\mathcal{C}}(x)$ | Kittenlab L8, L10 |

## Structure

- $\mathcal{C}$-sets and [[Natural Transformation|natural transformations]] (instance homomorphisms) form the [[Functor Category]] $\mathbf{Set}^{\mathcal{C}}$; it has all [[Limit|limits]] and [[Colimit|colimits]], computed pointwise — e.g. [[Coproduct|coproducts]] and [[Product|products]] of graphs are vertex-wise and edge-wise (Kittenlab Lectures 8, 13), [[Pushout|pushouts]] glue graphs (Lecture 9). It is a [[Topos]] (7 Sketches §7.3.1).
- The [[Yoneda Lemma]] says $F(x) \cong \mathrm{Hom}(y_x, F)$: elements of a table are maps out of the representable (Kittenlab Lecture 12: vertices of $G$ = maps from the one-vertex graph).
- [[Data Migration Functor|Data migration]]: a functor $F : \mathcal{C} \to \mathcal{D}$ between schemas induces $\Delta_F$ (pullback/precomposition) with adjoints $\Sigma_F \dashv \Delta_F \dashv \Pi_F$.
- Functors out of a [[Free Category|path category]] are stored as one set per vertex and one function per edge (Kittenlab), which is exactly what Catlab's `@acset_type` generates: a struct of tables with integer foreign keys. "Because a $\mathcal{C}$-set is a functor, all the constraints are ensured by the rules of functors" (7 Sketches).

````tabs
tab: Julia
```julia
# Kittenlab src/Diagrams.jl: a functor out of a finitely presented category, stored per generator
struct Diagram{L, Ob, Hom, C<:Category{Ob, Hom}} <: Functor{FinCat{L}, C}
  diagram::FinCat{L}; base::C
  ob_map::Dict{L, Ob}; hom_map::Dict{L, Hom}
end
Functors.ob_map(d::Diagram{L}, x::L) where {L} = d.ob_map[x]
function Functors.hom_map(d::Diagram{L}, x::FinCatMorphism{L}) where {L}
  foldl((f, g) -> compose(d.base, f, g), map(l -> d.hom_map[l], x.path); init = id(d.base, ob_map(d, x.dom)))
end

# Catlab: ACSets — the schema is presented, the instance is a struct of tables
using Catlab
@present SchDDS(FreeSchema) begin
  State::Ob
  next::Hom(State, State)
end
@acset_type DDS(SchDDS)
I = @acset DDS begin State = 7; next = [4, 4, 5, 5, 5, 7, 6] end     # Eq. (3.65)
I[:next]                    # the column
subpart(I, 3, :next)        # 5

# Example 3.46-style: check a path equation on an instance (s ⋅ s == s)
S = [1, 2, 2, 3, 3]; all(S[S[i]] == S[i] for i in eachindex(S))     # true: idempotent
```
tab: Lean
```lean
-- a C-set is a functor C ⥤ Type; the category of C-sets is the functor category
example (C : Type) [CategoryTheory.Category C] : Type _ := C ⥤ Type
example (C : Type) [CategoryTheory.Category C] : CategoryTheory.Category (C ⥤ Type) := inferInstance
-- Graphs as functors: Mathlib's `Quiver` is the "hand-rolled" version
```
tab: Haskell
```haskell
-- a C-set on the graph schema, stored one table per object and one function per generator
data GraphInst = GraphInst
  { vertices :: [Int]
  , edges    :: [Int]
  , srcCol   :: Int -> Int
  , tgtCol   :: Int -> Int
  }

-- a DDS-instance: a set with an endofunction
data DDS = DDS { states :: [Int], next :: Int -> Int }
dds365 :: DDS
dds365 = DDS [1..7] (\s -> [4,4,5,5,5,7,6] !! (s - 1))
```
````
