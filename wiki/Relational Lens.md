#definition #theorem #example #program

The **view-update problem**: a database exposes a *view* $V = \mathrm{get}(S)$ computed from a source $S$ by a query; a user edits the view; what should happen to the source? A **relational lens** answers by pairing the query with an update-translation

$$
\mathrm{get} : S \to V, \qquad \mathrm{put} : V \times S \to S,
$$

a [[Lens]] in the category of sets whose objects are database instances. $\mathrm{put}$ receives the old source because the view has *forgotten* something — the hidden columns of a projection, the filtered-out rows of a selection — and the lens has to decide how to restore it. A lens is **well-behaved** when

$$
\textbf{GetPut: } \mathrm{put}(\mathrm{get}(s), s) = s, \qquad \textbf{PutGet: } \mathrm{get}(\mathrm{put}(v, s)) = v,
$$

(writing back an unchanged view changes nothing; every update to the view is reflected exactly), and **very well-behaved** if also **PutPut**: $\mathrm{put}(v', \mathrm{put}(v, s)) = \mathrm{put}(v', s)$ — the last write wins, no history leaks through.

> Sources: Bancilhon & Spyratos, *Update semantics of relational views*, ACM TODS 6(4) (1981) (constant complements: the original categorical-flavoured answer); Foster, Greenwald, Moore, Pierce & Schmitt, *Combinators for bidirectional tree transformations: a linguistic approach to the view-update problem*, ACM TOPLAS 29(3) (2007) (lenses, the well-behavedness laws); Bohannon, Pierce & Vaughan, *Relational lenses: a language for updatable views*, PODS 2006 (select, project and join lenses with functional dependencies); Johnson, Rosebrugh & Wood, *Lenses, fibrations and universal translations*, Math. Struct. Comp. Sci. 22 (2012) (lenses as algebras for a monad; the universal-property view). See [[Lens]] for the general categorical definition and [[Profunctor Optics]] for the generalisations.

## Constant complements

Bancilhon and Spyratos' answer predates lenses. A view $V = \mathrm{get}(S)$ has a **complement** $C$ if $S \cong V \times C$ — the source is determined by the view together with the complement. An update to the view is translated by **keeping the complement constant**: $\mathrm{put}(v, s) = (v, c(s))$. For a projection of a table with a key, the complement is "the hidden columns, indexed by key". Constant-complement lenses are exactly the very well-behaved ones — the lens is a product projection, and $\mathrm{put}$ replaces one factor. Most interesting views have no constant complement, which is why practical lenses settle for well-behaved and give up PutPut.

## Lenses in the categorical sense

A very well-behaved lens $S \to V$ is the same as a product decomposition $S \cong V \times C$; a well-behaved lens is weaker. Johnson, Rosebrugh and Wood show that **lenses are the algebras of a monad** on the slice $\mathbf{Cat}/V$ and that they are a special case of split opfibrations (*delta lenses*, where $\mathrm{put}$ takes an update arrow in $V$ rather than a new state). This places the view-update problem next to the [[Grothendieck Construction]]: a lens says how to *lift* a change in the base (the view) to a change in the total space (the source).

## Combinators

Relational lenses are built compositionally: lenses compose (if $\mathrm{get}_1 ; \mathrm{get}_2$ is the view of a view, $\mathrm{put}$ composes by threading the intermediate state), and Bohannon–Pierce–Vaughan give primitive lenses for selection, projection (with a default for hidden columns, made sound by functional dependencies) and join (with a policy for deleting from one side or both). The laws of each primitive, plus composition, give the laws of every composite — the same move as [[Gradient-Based Learning with Parametric Lenses|lens-based learning]], where backpropagation is the $\mathrm{put}$ of a composite lens.

## Sophia

Sophia separates the immutable core terms (the source) from several *views* of them: names (`NAMED` edges), surface syntax per frontend, documentation. Editing a view — renaming, editing source text in an editor — must be translated back into new core nodes and new pointers. That is a lens problem with a good complement: the hashes. Renaming changes only the mutable `Name` relation and leaves the content-addressed complement constant, which is why it is cheap and PutPut-safe; editing surface syntax is a genuine `put` through elaboration. See [Naming and Change Propagation](https://mathstruct.org/Sophia/vault/Design/Naming-and-Change-Propagation).

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# A table Emp(name, dept, salary) with key `name`, and the view "names and departments" (a projection).
Row = NamedTuple{(:name, :dept, :salary),Tuple{String,String,Int}}
src = Set{Row}([(name = "ada", dept = "R&D", salary = 10), (name = "bob", dept = "Ops", salary = 7)])
get(s) = Set((name = r.name, dept = r.dept) for r in s)
# put: write an edited view back. Kept rows keep their hidden salary (looked up by the key);
# new rows get a default — the lens must decide what the view cannot see.
function put(v, s; default = 0)
    old = Dict(r.name => r.salary for r in s)
    Set{Row}((name = x.name, dept = x.dept, salary = Base.get(old, x.name, default)) for x in v)
end
# the well-behavedness laws of a lens
put(get(src), src) == src                         # GetPut: writing back an unchanged view changes nothing
v = Set([(name = "ada", dept = "Ops"), (name = "cy", dept = "R&D")])   # move ada, delete bob, add cy
get(put(v, src)) == v                             # PutGet: the update is reflected exactly in the view
sort([r.salary for r in put(v, src)])             # [0, 10]: ada keeps 10, the new row gets the default
# PutPut (very well-behaved) fails: deleting bob and re-adding him loses his salary.
v1 = Set([(name = "ada", dept = "R&D")]); v2 = get(src)
put(v2, put(v1, src)) == put(v2, src)             # false: the second put cannot recover bob's salary
```
tab: Lean
```lean
import Mathlib
-- A lens with its well-behavedness laws.
structure WBLens (S V : Type*) where
  get : S → V
  put : V → S → S
  getPut : ∀ s, put (get s) s = s
  putGet : ∀ v s, get (put v s) = v

-- Constant complement: S ≃ V × C gives a very well-behaved lens (PutPut holds).
def ofProduct {V C : Type*} : WBLens (V × C) V where
  get := Prod.fst
  put v s := (v, s.2)
  getPut _ := rfl
  putGet _ _ := rfl

example {V C : Type*} (v v' : V) (s : V × C) :
    ofProduct.put v' (ofProduct.put v s) = ofProduct.put v' s := rfl
```
tab: Haskell
```haskell
-- A projection lens on a keyed table: get hides the salary, put restores it by key.
import Data.List (sort)

type Row = (String, String, Int)        -- (name, dept, salary)

get :: [Row] -> [(String, String)]
get = sort . map (\(n, d, _) -> (n, d))

put :: [(String, String)] -> [Row] -> [Row]
put v s = sort [ (n, d, maybe 0 id (lookup n [ (n', sal) | (n', _, sal) <- s ])) | (n, d) <- v ]

main :: IO ()
main = do
  let src = sort [("ada", "R&D", 10), ("bob", "Ops", 7)]
  print (put (get src) src == src)                 -- GetPut: True
  let v = [("ada", "Ops"), ("cy", "R&D")]
  print (get (put v src) == sort v)                -- PutGet: True
```
````
