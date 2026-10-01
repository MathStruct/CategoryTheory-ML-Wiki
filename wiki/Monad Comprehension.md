#definition #theorem #example #program

A **monad comprehension** is the notation $[\, e \mid x \leftarrow E,\; y \leftarrow F(x),\; p(x, y) \,]$ read in an arbitrary [[Monad]] $T$ with a zero, where generators desugar to $\mathrm{bind}$ ($\mu \circ T f$), the result to $\mathrm{return}$ ($\eta$), and guards to the zero:

$$
[\, e \mid x \leftarrow E,\, Q \,] = E \mathbin{>\!\!>\!\!=} \lambda x.\, [\, e \mid Q \,], \qquad
[\, e \mid p,\, Q \,] = \text{if } p \text{ then } [\, e \mid Q \,] \text{ else } 0, \qquad
[\, e \mid \,] = \eta(e).
$$

Wadler observed that list comprehensions work for every monad; database theorists observed that **query languages are comprehensions over collection monads**. Select–from–where *is* a comprehension, and choosing the monad chooses the semantics: lists (ordered, with duplicates), bags (SQL), sets (relational algebra), or probability distributions (probabilistic databases).

> Sources: Wadler, *Comprehending monads*, Math. Struct. Comp. Sci. 2(4) (1992) (LFP 1990); Buneman, Naqvi, Tannen & Wong, *Principles of programming with complex objects and collection types*, Theor. Comput. Sci. 149 (1995) (the nested relational calculus, structural recursion on collections); Grust, *Monad comprehensions: a versatile representation for queries*, in *The Functional Approach to Data Management* (Springer 2004); Gibbons, *Comprehending ringads*, in *A List of Successes That Can Change the World* (LNCS 9600, 2016) (collection monads with $0$ and $+$; why bags and sets need more than a monad); Peyton Jones & Wadler, *Comprehensive comprehensions*, Haskell Workshop 2007 (order by, group by). DaoFP Ch. 14 for monads, [[Do Notation]] for the desugaring.

## Collection monads and ringads

The monads that model collections are **ringads**: monads with an empty collection $0$ and a union $+$ forming a monoid, with $\mathrm{bind}$ distributing over both. The monoid laws pick the collection type:

| $+$ is | collection | query semantics | as the free … |
|---|---|---|---|
| associative | list | ordered, duplicates | [[Free Monoid]] |
| + commutative | bag (multiset) | SQL | free commutative monoid |
| + idempotent | set | relational algebra | free semilattice ([[Power Set Monad]], finite) |
| weighted, normalised | distribution | probabilistic queries | [[Distribution Monad]] |

Moving between them is a **monad morphism** ($\mathrm{list} \to \mathrm{bag} \to \mathrm{set}$ forgets order, then multiplicity), and comprehensions commute with monad morphisms — a monad-level version of the [[Provenance Semiring|provenance]] theorem that query evaluation commutes with semiring homomorphisms. Indeed the bag monad annotates elements with $\mathbb N$ and the set monad with $\mathbb B$, and the semiring of annotations is what changes.

## Normalisation is the monad laws

Nested comprehensions can be flattened by the monad laws — associativity of $\mu$ turns "a query over the result of a query" into one comprehension over base tables, which is a join. The normal form theorem of the nested relational calculus says every query that returns flat relations normalises to a union of flat select–from–where blocks, so the nested, compositional language costs nothing at runtime. This is how LINQ, Links and Spark SQL compile language-integrated queries to SQL.

## Comprehensions and the Kleisli category

A comprehension is a composite in the [[Kleisli Category]] of the monad: each generator is a Kleisli arrow $x \mapsto F(x)$, and the comprehension composes them. Guards are composition with a partial identity (the zero). The [[Conjunctive Query|conjunctive queries]] are the comprehensions over the *set* monad that use only generators, equality guards and tuple results — the regular fragment again.

## Sophia

Sophia's queries are written in SQL, Cypher or Datalog over one store, and its [Query Cookbook](https://mathstruct.org/Sophia/vault/Design/Query-Cookbook) mixes set semantics (reachability), bag semantics (counting uses) and ordered results (shortest witness chains). Comprehensions are the common core: the same query, read in different collection monads.

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# A comprehension  [ (e.name, d.city) | e ← Emp, d ← Dept, e.dept == d.id ]  desugars to
# bind (flatMap), return (singleton) and zero (empty): the monad operations plus a zero.
Emp  = [(name = "ada", dept = 1), (name = "bob", dept = 2), (name = "cy", dept = 1)]
Dept = [(id = 1, city = "Paris"), (id = 2, city = "Oslo")]
bind(xs, f) = reduce(vcat, map(f, xs); init = [])          # list monad: μ ∘ T f
ret(x) = [x]; zero = []
q = bind(Emp, e -> bind(Dept, d -> e.dept == d.id ? ret((e.name, d.city)) : zero))
q                                                           # [("ada","Paris"), ("bob","Oslo"), ("cy","Paris")]
# The same comprehension in other collection monads changes the query semantics:
cities_list = bind(q, p -> ret(p[2]))                       # list: order and duplicates kept
bag = Dict{String,Int}(); for c in cities_list; bag[c] = Base.get(bag, c, 0) + 1; end
bag                                                         # bag (SQL): Paris => 2, Oslo => 1
Set(cities_list)                                            # set (relational algebra): {"Paris", "Oslo"}
# Normalisation: a nested comprehension flattens by the monad laws (μ associativity), giving a join.
nested = bind(bind(Emp, e -> ret(e.dept)), x -> bind(Dept, d -> d.id == x ? ret(d.city) : zero))
nested == bind(Emp, e -> bind(Dept, d -> d.id == e.dept ? ret(d.city) : zero))   # true
```
tab: Lean
```lean
import Mathlib
-- A comprehension in the List monad, written with do-notation; guards are `if … then … else []`.
def emp : List (String × Nat) := [("ada", 1), ("bob", 2), ("cy", 1)]
def dept : List (Nat × String) := [(1, "Paris"), (2, "Oslo")]

def joined : List (String × String) := do
  let (name, d) ← emp
  let (id, city) ← dept
  if d = id then pure (name, city) else []

example : joined = [("ada", "Paris"), ("bob", "Oslo"), ("cy", "Paris")] := by decide
```
tab: Haskell
```haskell
-- The same query as a list comprehension and in do-notation (the list monad).
emp :: [(String, Int)]
emp = [("ada", 1), ("bob", 2), ("cy", 1)]

dept :: [(Int, String)]
dept = [(1, "Paris"), (2, "Oslo")]

viaComprehension, viaDo :: [(String, String)]
viaComprehension = [ (n, c) | (n, d) <- emp, (i, c) <- dept, d == i ]
viaDo = do
  (n, d) <- emp
  (i, c) <- dept
  if d == i then return (n, c) else []

main :: IO ()
main = print (viaComprehension == viaDo, viaComprehension)
-- (True,[("ada","Paris"),("bob","Oslo"),("cy","Paris")])
```
````
