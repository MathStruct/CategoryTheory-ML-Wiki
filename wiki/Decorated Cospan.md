#definition #theorem #example

Let $\mathcal{C}$ have finite [[Colimit|colimits]] and $(F, \varphi) : (\mathcal{C}, +) \to (\mathbf{Set}, \times)$ be a [[Monoidal Functor|symmetric monoidal functor]], the **decoration functor**. An **$F$-decorated cospan** is a [[Cospan]] $A \xrightarrow{i} N \xleftarrow{o} B$ in $\mathcal{C}$ together with an element $s \in F(N)$, the **decoration**. Intuition ($\mathcal{C} = \mathbf{FinSet}$): $F(N)$ is the set of *legal decorations* on a set $N$ of nodes — e.g. all circuit diagrams with vertex set $N$ — and $A, B$ are the left and right external **ports** (terminals) mapping into the nodes.

**Composition.** Given $(A \xrightarrow{f} N \xleftarrow{g} B, s)$ and $(B \xrightarrow{h} P \xleftarrow{k} C, t)$, compose the cospans by [[Pushout]] and decorate the new apex $N +_B P$ with

$$
F([\iota_N, \iota_P])\big(\varphi_{N,P}(s, t)\big) \in F(N +_B P): \qquad (6.76)
$$

first put the two decorations side by side with $\varphi_{N,P}$, then glue along the identifications specified by $B$ using the copairing of the pushout maps. Monoidal product: coproduct cospans decorated by $\varphi_{M,N}(s, t)$ — "stacking".

**Theorem 6.77.** There is a [[Hypergraph Category]] $\mathbf{Cospan}_F$ with the objects of $\mathcal{C}$ and morphisms (equivalence classes of) $F$-decorated cospans; its symmetric monoidal and hypergraph structures come from $\mathbf{Cospan}_{\mathcal{C}}$. With the constant functor $F(c) = \{\ast\}$ one recovers $\mathbf{Cospan}_{\mathcal{C}}$ ([[7S Exercise 6.78]]).

> Sources: 7 Sketches §6.4.2–6.4.3 (Definition 6.75, Eq. 6.76, Theorem 6.77, Exercises 6.78–6.88), §6.6; [Fon15; Fon18; BF15; BFP16; BP17]; generalization: [[Structured Cospan|structured cospans]] (Catlab).

## Open electric circuits (§6.4.3)

Fix a set of components $C := \{\mathsf{light}, \mathsf{switch}, \mathsf{battery}\} \sqcup \{x\Omega \mid x \in \mathbb{R}_+\}$. A **$C$-circuit** is a [[Graph]] $(V, A, s, t)$ with a labelling $\ell : A \to C$ (edge directions are an artifact of the representation; [[7S Exercise 6.79]]). The decoration functor $\mathbf{Circ} : (\mathbf{FinSet}, +) \to (\mathbf{Set}, \times)$ sends $V$ to the set of $C$-circuits on $V$, and $f : V \to V'$ to $(V, A, s, t, \ell) \mapsto (V', A, s \mathbin{;} f, t \mathbin{;} f, \ell)$ — merging nodes ([[7S Exercise 6.80]]); $\psi_{V,V'}$ takes the disjoint union of two circuits (Eq. 6.81, [[7S Exercise 6.82]]). Morphisms $m \to n$ of $\mathbf{Cospan}_{\mathbf{Circ}}$ are **open circuits**: a cospan $\underline{m} \to \underline{p} \leftarrow \underline{n}$ plus a circuit on $p$ nodes, e.g. the battery $1 \to 2 \leftarrow 1$ (Eq. 6.83, [[7S Exercise 6.84]]). Composing two open circuits $1 \to 1$ pushes out over the shared terminal and glues the circuits ([[7S Exercise 6.86]] recomputes Eq. 6.74); monoidal product stacks (Eq. 6.87); closing off with the Frobenius cup $\eta : 0 \to 2$ and cap $\varepsilon : 2 \to 0$ decorated by empty circuits gives a **closed circuit** $0 \to 0$ ([[7S Exercise 6.88]]) — the light-switch circuit of §6.1.

Decorated cospans yield "an explicit category equipped with Frobenius structures that get around the strictures of domains and codomains"; [[Operad Algebra|$\mathbf{Cospan}$-algebras]] are more general (they produce *every* hypergraph prop), and the functor $\mathbf{Circ} : \mathbf{Cospan} \to \mathbf{Set}$ is such an algebra (Example 6.100). Circuit *semantics* (e.g. the relation between boundary potentials and currents for passive linear circuits) is a further hypergraph functor to $\mathbf{LinRel}$ [BF15].

````tabs
tab: Julia
```julia
using Catlab
# Catlab implements decorated / structured cospans; open graphs and open Petri nets are built-in examples.
# Open circuits as structured cospans of labelled graphs: the "decoration" is the graph on the apex.
@present SchCircuit <: SchGraph begin
  Label::AttrType
  label::Attr(E, Label)
end
@acset_type Circuit(SchCircuit, index=[:src, :tgt])
const OpenCircuitOb, OpenCircuit = OpenACSetTypes(Circuit, :V)      # feet are finite sets mapping to nodes
battery = @acset Circuit{Symbol} begin V = 2; E = 1; src = [1]; tgt = [2]; label = [:battery] end
open_battery = OpenCircuit{Symbol}(battery, FinFunction([1], 2), FinFunction([2], 2))   # 1 → 2 ← 1, Eq. (6.83)
resistor = @acset Circuit{Symbol} begin V = 2; E = 1; src = [1]; tgt = [2]; label = [:ohm5] end
open_res = OpenCircuit{Symbol}(resistor, FinFunction([1], 2), FinFunction([2], 2))
composite = compose(open_battery, open_res)          # glue along the shared terminal (pushout)
apex(composite)                                       # a Circuit with 3 nodes and 2 labelled edges
```
tab: Haskell
```haskell
-- an F-decorated cospan: a cospan of finite sets with a decoration on the apex
data DecoratedCospan dec = DecoratedCospan
  { leftLeg :: [Int], rightLeg :: [Int]   -- functions A → N, B → N as lists of node indices
  , nodes :: Int, decoration :: dec }
-- a C-circuit decoration on n nodes: labelled edges
data Circuit = Circuit { edges :: [(Int, Int, String)] }
-- composition: pushout of the middle feet, then relabel node indices in both circuits and take the union
```
````
