#definition #theorem #algorithm #program

An **e-graph** is a finite data structure representing a (usually infinite) set of terms together with an equivalence relation on them that is a **congruence**: if $a \equiv b$ then $f(a) \equiv f(b)$. Concretely (Willsey et al., *egg*, Definition 2.1) it is a triple $(U, M, H)$ of

- a **union-find** $U$ storing an equivalence relation $\equiv_{\mathrm{id}}$ on **e-class ids**;
- an **e-class map** $M$ sending each id to its **e-class**, a set of **e-nodes** $f(a_1, \dots, a_k)$ — a function symbol applied to e-class ids, not to terms;
- a **hashcons** $H$ from e-nodes to e-class ids, so that no e-node is stored twice.

An e-class *represents* a term $f(t_1, \dots, t_k)$ if it contains an e-node $f(a_1, \dots, a_k)$ whose children $a_i$ represent the $t_i$ (Definition 2.3). Two invariants make this a congruence rather than just an equivalence: the **congruence invariant** (equal children ⇒ equal e-classes, Definition 2.6) and the **hashcons invariant** (Definition 2.7). After a batch of merges, **rebuilding** restores both by re-canonicalising e-nodes and merging the classes that now collide — the deferred-maintenance idea that made e-graphs fast.

> Sources: Willsey, Nandi, Wang, Flatt, Tatlock & Panchekha, *egg: Fast and Extensible Equality Saturation*, POPL 2021, [arXiv:2004.03082](https://arxiv.org/abs/2004.03082) ([[egg - Fast and Extensible Equality Saturation|notes]]) Definitions 2.1–2.7, §3 (rebuilding), §4 (e-class analyses); Suciu, Wang & Zhang, *Semantic Foundations of Equality Saturation*, ICDT 2025, [arXiv:2501.02413](https://arxiv.org/abs/2501.02413) ([[Semantic Foundations of Equality Saturation|notes]]) Definitions 3, 5, 13, Theorems 6, 19, 30–33, Corollary 27; Zhang, Wang, Willsey & Tatlock, *Relational E-Matching*, POPL 2022, [arXiv:2108.02290](https://arxiv.org/abs/2108.02290) ([[Relational E-Matching|notes]]) Definitions 3, 6, Theorems 9–10; Zhang et al., *Better Together: Unifying Datalog and Equality Saturation*, PLDI 2023, [arXiv:2304.04332](https://arxiv.org/abs/2304.04332) ([[Better Together - Unifying Datalog and Equality Saturation|notes]]) §§3–4, Theorem 4.1. Background: Nelson & Oppen, *Fast decision procedures based on congruence closure*, JACM 27 (1980); Tate, Stepp, Tatlock & Lerner, *Equality saturation: a new approach to optimization*, POPL 2009.

## What an e-graph means: a quotient of the term algebra

For a signature $\Sigma$, the terms $T(\Sigma)$ form the [[Initial Algebra]] of the polynomial functor $X \mapsto \coprod_{f \in \Sigma} X^{\mathrm{ar}(f)}$. A **congruence** on an algebra is an equivalence relation that is also a subalgebra of $A \times A$ — equivalently, the kernel of a homomorphism — and quotienting by it gives another algebra ([[Congruence]]).

Suciu, Wang and Zhang make the folklore precise. An e-graph **is** a deterministic, reachable *tree automaton* $G = \langle Q, \Sigma, \Delta \rangle$ without final states (Definition 3): states are e-classes, transitions $f(c_1, \dots, c_k) \to c$ are e-nodes, and a term is represented by $c$ iff the automaton accepts it at $c$. Equivalently, $G$ is a finite **partial $\Sigma$-algebra**, and "the e-class of $t$" is the unique (partial) homomorphism out of the initial algebra. Its semantics is the induced **partial congruence relation** $\approx_G$ (Definition 5) — symmetric, transitive, congruent and closed under subterms — and the correspondence is exact:

> **Theorem 6.** For every partial congruence $\approx$ on $T(\Sigma)$ there is a unique e-graph $G$ with $\approx_G \;=\; \approx$.

So an e-graph is to a congruence what a minimal automaton is to a regular language: a canonical finite presentation. E-graph homomorphisms, when they exist, are unique (Lemma 11) and order e-graphs by "represents more equalities".

## Equality saturation is a least fixed point

Given a set $R$ of rewrite rules $u \approx v$, **equality saturation** repeatedly matches a left-hand side, adds the right-hand side, merges, and rebuilds — never deleting anything. Semantically this is iteration of a monotone, inflationary operator on e-graphs ordered by $\sqsubseteq$, and the result is its least fixed point ([[Least Fixed Point]]):

> **Theorem 19.** The immediate-consequence-and-rebuild operator has a least fixed point above $G$, namely $\mathrm{EqSat}(R, G) = \bigsqcup_{i \ge 0} \mathrm{ICOR}^{(i)}(G)$, and it is a **universal model** of $R$ and $G$ (Definition 13).

"Universal model" is the database phrasing of an initial object: the saturated e-graph maps uniquely into every e-graph that contains $G$ and satisfies $R$. Equality saturation is the **chase** of database theory in disguise: rules become tuple- and equality-generating dependencies, and EqSat terminates iff the corresponding chase does (Theorem 30). Termination is R.E.-complete for one instance, $\Pi_2$-complete for all terms and undecidable for all e-graphs (Theorems 31–33), which is why every practical implementation runs with node, time and iteration budgets.

## An e-graph is a database

Relational e-matching stores each function symbol $f$ of arity $k$ as a table $R_f(\mathit{id}, a_1, \dots, a_k)$ and compiles a pattern into a [[Conjunctive Query]]; the pattern $f(\alpha, g(\alpha))$ becomes $Q(\mathit{root}, \alpha) \leftarrow R_f(\mathit{root}, \alpha, x), R_g(x, \alpha)$. Structural constraints and equality constraints become the same thing — join conditions — and worst-case optimal join algorithms make e-matching worst-case optimal (Theorem 9).

**egglog** closes the loop: a Datalog engine whose tables are *functions* with a functional dependency from arguments to result, plus a `:merge` expression for conflicts. When the merge is "union the two e-classes", restoring the functional dependency *is* rebuilding, so equality saturation is Datalog with a union-find; when the merge is a lattice join, it is an e-class analysis. Semi-naïve evaluation remains correct (Theorem 4.1).

## E-class analyses are abstract interpretation

An **e-class analysis** (egg §4) attaches to every e-class a fact $d_c$ from a join-semilattice $D$, via `make` on e-nodes, `join` on merges and an idempotent `modify`. The invariant is $d_c = \bigvee_{n \in c} \mathrm{make}(n)$: the fact for a class is the join over all its representations. Constant folding, free-variable sets and interval bounds are all analyses; because they are lattice-valued, merging classes in any order gives the same result — the same reason Datalog over a lattice has a well-defined least fixed point.

## E-graphs and hash consing

Hash-consing a term gives one shared node per *structurally identical* subterm; an e-graph gives one *class* per set of *semantically equivalent* subterms. The hashcons inside an e-graph does the first job, the union-find the second. In practice the second can explode: associativity and commutativity rules alone generate exponentially many e-nodes, which is why saturation runs under budgets and why AC is often handled specially rather than by rewrite rules.

## Extraction

The output of a saturation run is a choice of one e-node per reachable class, minimising a cost — a weighted, acyclicity-constrained selection that is NP-hard in general. Greedy bottom-up extraction is exact for tree-additive costs; integer programming handles shared subterms.

## Sophia

Sophia's compiler *is* saturate-then-extract over a persistent store: rewrite rules emit `EQUIV` edges instead of replacing nodes, rules are stored nodes, and extraction picks an implementation per target. The append-only graph store is an e-graph whose hashcons is the content hash. See [Compilation as Query](https://mathstruct.org/Sophia/vault/Design/Compilation-as-Query) and [Equivalence and Witnesses](https://mathstruct.org/Sophia/vault/Design/Equivalence-and-Witnesses).

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# A minimal e-graph (Willsey et al., Definitions 2.1–2.7): union-find + hashcons + rebuild.
mutable struct EGraph
    parent::Vector{Int}                          # union-find over e-class ids
    hashcons::Dict{Tuple{Symbol,Vector{Int}},Int} # canonical e-node ↦ e-class id
end
EGraph() = EGraph(Int[], Dict{Tuple{Symbol,Vector{Int}},Int}())
find(g, a) = g.parent[a] == a ? a : (g.parent[a] = find(g, g.parent[a]))
canon(g, (f, args)) = (f, [find(g, a) for a in args])
function add!(g, f::Symbol, args::Int...)
    n = canon(g, (f, collect(args)))
    haskey(g.hashcons, n) && return find(g, g.hashcons[n])   # hashcons: no duplicate e-nodes
    push!(g.parent, length(g.parent) + 1)
    g.hashcons[n] = length(g.parent)
end
function union!(g, a, b)
    a, b = find(g, a), find(g, b)
    a != b && (g.parent[b] = a)
    a
end
function rebuild!(g)                          # restore the congruence invariant
    changed = true
    while changed
        changed = false
        new = Dict{Tuple{Symbol,Vector{Int}},Int}()
        for (n, c) in g.hashcons
            n2, c2 = canon(g, n), find(g, c)
            if haskey(new, n2) && find(g, new[n2]) != c2
                union!(g, new[n2], c2); changed = true   # congruence: equal args ⇒ equal results
            end
            new[n2] = find(g, c2)
        end
        g.hashcons = new
    end
    g
end
g = EGraph()
a, b = add!(g, :a), add!(g, :b)
fa, fb = add!(g, :f, a), add!(g, :f, b)
ffa = add!(g, :f, fa)
find(g, fa) == find(g, fb)          # false: nothing asserted yet
union!(g, a, b); rebuild!(g)
find(g, fa) == find(g, fb)          # true: a ≡ b forces f(a) ≡ f(b)
union!(g, fa, a); rebuild!(g)       # assert f(a) = a
find(g, ffa) == find(g, a)          # true: f(f(a)) ≡ f(a) ≡ a, by congruence
length(unique(find(g, c) for c in values(g.hashcons)))   # 1 e-class left: {a, b, f(a), f(b), f(f(a))}
```
tab: Lean
```lean
import Mathlib
-- A congruence for one unary operation: an equivalence relation (Mathlib's `Setoid`) respected by f.
def IsCongruence {α : Type*} (r : Setoid α) (f : α → α) : Prop :=
  ∀ a b, r a b → r (f a) (f b)

-- What an e-graph's rebuild guarantees: a ≈ b forces f (f a) ≈ f (f b).
example {α : Type*} (r : Setoid α) (f : α → α) (h : IsCongruence r f) (a b : α)
    (hab : r a b) : r (f (f a)) (f (f b)) :=
  h _ _ (h _ _ hab)

-- Quotienting by a congruence: f descends to the quotient (Mathlib's `Quotient.map`).
def descend {α : Type*} (r : Setoid α) (f : α → α) (h : IsCongruence r f) :
    Quotient r → Quotient r :=
  Quotient.map f h
```
tab: Haskell
```haskell
-- Congruence closure by naive saturation: e-nodes are (symbol, child classes).
type Class = Int
type ENode = (String, [Class])

-- one rebuild pass: canonicalise every e-node, merge classes whose e-nodes collide
rebuild :: (Class -> Class) -> [(ENode, Class)] -> [(Class, Class)]
rebuild find nodes =
  [ (c1, c2) | ((n, c1), i) <- zip canon [0 :: Int ..], ((n', c2), j) <- zip canon [0 ..]
             , i < j, n == n', find c1 /= find c2 ]
  where canon = [ ((f, map find as), find c) | ((f, as), c) <- nodes ]

-- e-graph {a:1, b:2, f(a):3, f(b):4} with a ≡ b asserted: rebuild finds f(a) ≡ f(b)
main :: IO ()
main = print (rebuild find [(("a", []), 1), (("b", []), 2), (("f", [1]), 3), (("f", [2]), 4)])
  where find c = if c == 2 then 1 else c       -- union-find after union(1, 2)
-- prints [(3,4)]: the new congruence f(a) ≡ f(b), forced by a ≡ b
```
````
