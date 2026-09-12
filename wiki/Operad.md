#definition #example

An **operad** $\mathcal{O}$ (also *multicategory* / *coloured operad*; Leinster [Lei04]) consists of

(i) a collection $T$ of **types** (objects, colours);
(ii) for each tuple $(t_1, \dots, t_n; t)$ of types, a set $\mathcal{O}(t_1, \dots, t_n; t)$ of **operations** of that **arity** — think "boxes with $n$ input ports of types $t_i$ and output type $t$", or functions of $n$ arguments;
(iii) **substitution** $\circ_i : \mathcal{O}(s_1, \dots, s_m; t_i) \times \mathcal{O}(t_1, \dots, t_n; t) \to \mathcal{O}(t_1, \dots, t_{i-1}, s_1, \dots, s_m, t_{i+1}, \dots, t_n; t)$: plug an $m$-ary operation into the $i$-th argument of an $n$-ary one to get an $(m + n - 1)$-ary operation;
(iv) identities $\mathrm{id}_t \in \mathcal{O}(t; t)$,

satisfying generalized associativity and identity laws. Operads are a "meta-compositional structure": whereas [[Free Prop|free]] and [[Presentation of a Prop|presented]] structures tailor *instances* of preorders, categories or props, operads tailor *the algebraic structures themselves*. "Making tea is a 2-ary operation: you need warm water and tea leaves."

> Sources: 7 Sketches §6.5 (Rough Definition 6.91, Examples 6.92–6.94, Definition 6.97, Rough Definition 6.98, Exercise 6.96), §6.6; [May72; Lei04; RS13; Spi13; VSL15]; Catlab (`oapply` for operad algebras on wiring diagrams).

## Examples

- **$\mathbf{Set}$** (Example 6.93): types are sets, operations $X_1 \times \cdots \times X_n \to Y$ are functions of $n$ variables, substitution plugs one function into an argument of another, identities are identity functions.
- **$\mathbf{Cospan}$** (Example 6.94): types $a \in \mathbb{N}$, operations of arity $(a_1, \dots, a_n; b)$ are [[Cospan|cospans]] $a_1 + \cdots + a_n \to p \leftarrow b$ in $\mathbf{FinSet}$, substitution by [[Pushout]], identity cospans. An operation is drawn as an [[Undirected Wiring Diagram]]: $n$ inner circles with $a_i$ ports, an outer circle with $b$ ports, and $p$ junction nodes; e.g. Eq. (6.95) is an operation of arity $(3, 3, 4, 2; 3)$ with apex $\underline{7}$. Substitution = **inserting one wiring diagram into a circle of another** ([[7S Exercise 6.96]]). This is the operadic analogue of $(\mathbf{Cospan}_{\mathbf{FinSet}}, 0, +)$ with the left/right distinction removed: "circuits just have a single boundary interface, not domains and codomains" (Eqs. 6.89–6.90).
- **From any [[Symmetric Monoidal Category]]** (Definition 6.97): the **underlying operad** $\mathcal{O}_{\mathcal{C}}$ has types $\mathrm{Ob}(\mathcal{C})$, operations $\mathcal{C}(C_1 \otimes \cdots \otimes C_n, D)$, substitution $f \circ_i g := f \circ (\mathrm{id}, \dots, g, \dots, \mathrm{id})$. Monoidal functors give operad functors.
- **Context-free grammars are to operads as graphs are to categories** (Example 6.92): syntactic categories (noun, determiner, noun phrase, sentence) are types, production rules are generating operations, and a grammar presents a free operad [HMP98].
- Operads of wiring diagrams with constraints (e.g. no "passing wires", needed for feedback in dynamical systems [VSL15]); the operad of cobordisms; "there is an operad for operads" ([Lei04, 2.2.23]).

**Operad functors** (Rough Definition 6.98): a map of types $f : T \to U$ and maps $\mathcal{O}(t_1, \dots, t_n; t) \to \mathcal{P}(f t_1, \dots, f t_n; f t)$ preserving substitution and identities. **[[Operad Algebra|Algebras]]** are operad functors $\mathcal{O} \to \mathbf{Set}$: ways to *fill the boxes* of a wiring-diagram grammar.

Operads conclude the "informal hierarchy of compositional structures: preorders, categories, monoidal categories, operads".

````tabs
tab: Julia
```julia
using Catlab, Catlab.WiringDiagrams
# operations of the operad Cospan as undirected wiring diagrams (Exercise 6.96)
f = UndirectedWiringDiagram(2)                # outer arity 2
add_box!(f, 2); add_box!(f, 2)                # two inner circles with two ports each
add_junctions!(f, 3)
set_junction!(f, [1, 2, 2, 3])                # box1 ports ↦ j1, j2 ; box2 ports ↦ j2, j3
set_junction!(f, [1, 3], outer=true)          # f ∈ Cospan(2, 2; 2)
g = UndirectedWiringDiagram(0)                # g ∈ Cospan(2, 2, 2; 0): a closed triangle
add_box!(g, 2); add_box!(g, 2); add_box!(g, 2)
add_junctions!(g, 3)
set_junction!(g, [1, 2, 2, 3, 3, 1])
gf = ocompose(g, 1, f)                        # substitution g ∘₁ f, arity (2, 2, 2, 2; 0)
nboxes(gf), length(ports(gf, outer=true))     # (4, 0)
```
tab: Lean
```lean
-- Mathlib does not (yet) have a general operad/multicategory library; the operad underlying a
-- symmetric monoidal category has operations Hom (C₁ ⊗ ⋯ ⊗ Cₙ) D.
```
tab: Haskell
```haskell
-- an untyped operad: n-ary operations with substitution ∘_i (schematic)
class Operad op where
  arity :: op -> Int
  identity :: op                      -- arity 1
  subst :: op -> Int -> op -> op      -- g ∘_i f: plug f into the i-th argument of g
-- the operad of functions: newtype Fn = Fn ([Double] -> Double) with arity tracked separately
```
````
