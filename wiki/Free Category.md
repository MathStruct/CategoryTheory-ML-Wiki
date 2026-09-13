#definition #example #theorem #proof

For any [[Graph]] $G = (V, A, s, t)$, the **free category** $\mathrm{Free}(G)$ (Kittenlab: the **path category** $\mathrm{Path}(G)$) has objects the vertices $V$ and morphisms $c \to d$ the [[Path in a Graph|paths]] from $c$ to $d$. The identity on $c$ is the trivial (length-0) path; composition is concatenation of paths. We often elide the difference between a graph and its free category.

> Sources: 7 Sketches Definition 3.7, Eq. (3.8), Example 3.13, Exercises 3.9, 3.10, 3.12, 3.15, 3.33; Remark 3.23; Kittenlab Lecture 6; DaoFP §8.1 (stick-figure categories), §10.9 (free constructions).

*Proof that it is a category* ([[7S Chapter 3 Exercises#Exercise 3.9|7S Exercise 3.9]]): define a path as $(v, a_1, \dots, a_n)$ with $s(a_1) = v$ and $t(a_i) = s(a_{i+1})$; concatenation $(v, a_1..a_m) \mathbin{;} (w, b_1..b_n) = (v, a_1, .., a_m, b_1, .., b_n)$ when $t(p) = w$. Concatenating with a length-0 path returns the same tuple (unitality), and either bracketing of three paths yields the same tuple (associativity). $\blacksquare$

## Examples

- $\underline{\mathbf{2}} = \mathrm{Free}(v_1 \xrightarrow{f_1} v_2)$: two objects, three morphisms $\mathrm{id}_{v_1}, f_1, \mathrm{id}_{v_2}$ — the [[Walking Arrow]].
- $\underline{\mathbf{3}} = \mathrm{Free}(v_1 \to v_2 \to v_3)$: three objects, six morphisms ([[7S Chapter 3 Exercises#Exercise 3.10|7S Exercise 3.10]]); $\underline{\mathbf{n}}$ has $1 + 2 + \cdots + n$ morphisms, $\underline{\mathbf{1}}$ has one object and one morphism, $\underline{\mathbf{0}}$ is empty ([[7S Chapter 3 Exercises#Exercise 3.12|7S Exercise 3.12]]).
- Example 3.13: the graph with one vertex $z$ and one loop $s$ has paths $z, s, s \mathbin{;} s, \dots$, one of each length: $\mathrm{Free}$ of it is the [[Natural Numbers]] $(\mathbb{N}, +, 0)$ as a one-object category, a [[Monoid]] (concatenation adds lengths, [[7S Chapter 3 Exercises#Exercise 3.15|7S Exercise 3.15]]).
- The free square category has ten morphisms; adding the equation $f \mathbin{;} h = g \mathbin{;} i$ gives the *commutative square* with nine ([[Presentation of a Category]]).
- The only isomorphisms in $\mathrm{Free}(G)$ are identities, since lengths add ([[7S Chapter 3 Exercises#Exercise 3.33|7S Exercise 3.33]]).

## Properties

- **Defining functors out of $\mathrm{Path}(G)$ is easy** (Kittenlab Lecture 6): pick an object $F(v)$ for every vertex and a morphism $F(e) : F(s(e)) \to F(t(e))$ for every edge; a path goes to the composite $F(e_n) \circ \cdots \circ F(e_1)$, and preservation of composition comes "for free". Such functors are easy to *store*: an object per vertex, a morphism per edge — this is Kittenlab's `Diagram` and Catlab's `FinDomFunctor`. Functors $\mathrm{Path}(G) \to \mathbf{Set}$ are [[C-Set|C-sets / ACSets]].
- $\mathrm{Free}$ is a [[Functor]] $\mathbf{Grph} \to \mathbf{Cat}$, left adjoint to the underlying-graph functor (7 Sketches Example 3.74, DaoFP "free functors generate structure freely and lazily"). Similarly graphs generate free [[Preorder|preorders]] ([[Hasse Diagram]]).
- Free categories and preorders are two ends of a spectrum (Remark 3.23): with no equations every path is a distinct morphism; with all equations, parallel paths are identified ([[Preorder Reflection]]). Every [[Presentation of a Category]] lies in between.
- The free category on a graph is the [[Free Monoid]] "with types": DaoFP's free monoid on an alphabet is the free category on a one-vertex graph.

````tabs
tab: Julia
```julia
# Kittenlab src/FinCats.jl: a finitely presented (here: free) category with paths as morphisms
struct FinCatMorphism{L}
  dom::L; codom::L; path::Vector{L}
end
struct FinCat{L} <: Category{L, FinCatMorphism{L}}
  objects::Set{L}
  homs::Dict{L, Tuple{L, L}}          # generator => (source, target)
end
Categories.dom(::FinCat, f::FinCatMorphism) = f.dom
Categories.codom(::FinCat, f::FinCatMorphism) = f.codom
function Categories.compose(::FinCat{L}, f::FinCatMorphism{L}, g::FinCatMorphism{L}) where {L}
  @assert f.codom == g.dom
  FinCatMorphism(f.dom, g.codom, L[f.path; g.path])
end
Categories.id(::FinCat{L}, x::L) where {L} = FinCatMorphism{L}(x, x, L[])

C = FinCat(Set([:a, :b, :c]), Dict(:f => (:a, :b), :g => (:b, :c)))

# Catlab: free category on a graph
using Catlab
g = @acset Graph begin V = 3; E = 2; src = [1, 2]; tgt = [2, 3] end
C = FinCat(g)
hom_generators(C)              # the two edges
```
tab: Lean
```lean
#check CategoryTheory.Paths        -- Paths V : the free category on a quiver V
#check CategoryTheory.Paths.of     -- the prefunctor V ⥤q Paths V
#check CategoryTheory.Paths.lift   -- a prefunctor V ⥤q C extends uniquely to Paths V ⥤ C
-- the adjunction Free ⊣ Forget between quivers and categories (Mathlib.CategoryTheory.Category.Quiv):
#check CategoryTheory.Quiv.adj     -- Cat.free ⊣ Quiv.forget
```
tab: Haskell
```haskell
-- the free category on a graph: morphisms are lists of edges (paths)
data Path e = Path [e]         -- with implicit source/target bookkeeping
instance Category' (Path e) where   -- sketch: composition is concatenation, identity is []
  idP = Path []
  Path q `after` Path p = Path (p ++ q)

-- DaoFP-style free structure: a free monoid is a list, the free category is a typed list
```
````
