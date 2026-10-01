#definition #theorem #example #program

Two program fragments $t$ and $u$ are **contextually equivalent** (Morris), written $t \simeq_{\mathrm{ctx}} u$, if no complete program can tell them apart:

$$
t \simeq_{\mathrm{ctx}} u \quad\Longleftrightarrow\quad \forall C[\cdot].\; \big( C[t] \Downarrow v \iff C[u] \Downarrow v \big),
$$

where $C$ ranges over all well-typed contexts — programs with a hole — and $\Downarrow v$ is the chosen **observation** (termination, or termination with a value of ground type). It is the natural notion of "these two do the same thing" for a programming language, and it has two defining properties: it is a [[Congruence]] (if $t \simeq u$ then $C[t] \simeq C[u]$, since contexts compose), and it is **adequate** (equivalent closed programs have the same observable result). In fact it is the **largest** adequate congruence. Everything finer — syntactic equality, $\beta\eta$-equality, equality by a set of rewrite rules — is contained in it; everything coarser, such as "agrees on a test suite", is not a congruence.

> Sources: Morris, *Lambda-calculus models of programming languages*, PhD thesis, MIT 1968; Milner, *Fully abstract models of typed λ-calculi*, Theor. Comput. Sci. 4 (1977); Plotkin, *LCF considered as a programming language*, Theor. Comput. Sci. 5 (1977) (the full abstraction problem for PCF); Pitts, *Operational semantics and program equivalence*, in *Applied Semantics* (LNCS 2395, 2002); Abramsky, Jagadeesan & Malacaria, *Full abstraction for PCF*, and Hyland & Ong, *On full abstraction for PCF*, Inform. and Comput. 163 (2000) (game semantics). Proof methods: [[Logical Relations]], [[Bisimulation]].

## Why it is hard to prove directly

The quantifier over *all* contexts is the problem: a context can do anything the language allows, including things the programmer never intended. Two functions that agree on every input may still be distinguishable if the language lets a context observe sharing, timing of effects, exceptions, or — as in the Julia tab — the order in which equal elements are returned. Proving $t \simeq_{\mathrm{ctx}} u$ therefore goes through a proxy that is a congruence by construction:

| method | idea |
|---|---|
| [[Logical Relations]] | a type-indexed relation that respects every construct; the fundamental lemma gives congruence |
| applicative / environmental [[Bisimulation]] | a coinductive relation on closed terms, shown to be a congruence (Howe's method) |
| fully abstract denotational model | a semantics in which $\llbracket t \rrbracket = \llbracket u \rrbracket \iff t \simeq_{\mathrm{ctx}} u$; equality is then computed in the model |

Finding a fully abstract model for PCF was open for two decades; game semantics solved it (2000). Categorically, a fully abstract model is a quotient of the syntactic category by contextual equivalence that is also given by some independent, compositional construction.

## The observation is a parameter

Changing what counts as observable changes the relation: if running time or the number of allocations is observable, far fewer programs are equivalent. An equivalence "modulo timing" is contextual equivalence for a coarser observation. And **across two languages the definition does not even type-check** — $C$ would have to be a context of *which* language? A cross-language relation (a multi-language semantics, or a translation into a common core) is needed before the question can be asked.

## Refinement, the one-sided version

Replacing $\iff$ by $\implies$ gives **contextual refinement** $t \sqsubseteq u$: every observation $t$ can make, $u$ can make too (or the converse, depending on convention — "fewer behaviours", "more defined"). Refinement is the right notion for implementing a specification, for nondeterministic code, and for moving to a language with a stricter discipline. It is a preorder, not an equivalence, and the [[Cartesian Bicategory|order of a cartesian bicategory]] of relations is its abstract form.

## Sophia

Sophia's `observational` equivalence level is contextual equivalence modulo a stated set of ignored observations (`alloc`, `timing`, `fp_assoc`, …), and its `REFINES` edge is contextual refinement. The design notes' cautionary example — two sorting functions that agree on every test but differ on stability — is the Julia tab below: `tested` equivalence is not a congruence, so it may not license substitution. See [Equivalence and Witnesses](https://mathstruct.org/Sophia/vault/Design/Equivalence-and-Witnesses).

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
using Random; Random.seed!(3)
# Two sorting functions by key: insertion sort (stable) and selection sort with swaps (unstable).
function stable_sort(xs; by = identity)
    out = similar(xs, 0)
    for x in xs
        i = findfirst(y -> by(y) > by(x), out)
        insert!(out, i === nothing ? length(out) + 1 : i, x)
    end
    out
end
function unstable_sort(xs; by = identity)
    a = copy(xs)
    for i in eachindex(a)
        j = i - 1 + argmin([by(y) for y in a[i:end]])
        a[i], a[j] = a[j], a[i]                       # the swap can jump equal keys past each other
    end
    a
end
# A test suite over integers cannot tell them apart …
tests = [rand(-9:9, rand(0:8)) for _ in 1:500]
all(stable_sort(t) == unstable_sort(t) for t in tests)                 # true
# … but a context that sorts records by key and then observes their order does:
C(sortfn) = [r[2] for r in sortfn([(2, :a), (1, :b), (2, :c), (1, :d)]; by = first)]
C(stable_sort)                                                         # [:b, :d, :a, :c]
C(unstable_sort)                                                       # [:b, :d, :c, :a]
C(stable_sort) == C(unstable_sort)                                     # false: not contextually equivalent
```
tab: Lean
```lean
import Mathlib
-- Two sorts by key: Mathlib's insertion sort (stable), and the same sort on the reversed list,
-- which puts records with equal keys in reverse order (unstable).
def keyLE (a b : ℕ × Char) : Prop := a.1 ≤ b.1
instance : DecidableRel keyLE := fun a b => inferInstanceAs (Decidable (a.1 ≤ b.1))
def stab (l : List (ℕ × Char)) := l.insertionSort keyLE
def unst (l : List (ℕ × Char)) := l.reverse.insertionSort keyLE

def recs : List (ℕ × Char) := [(2, 'a'), (1, 'b'), (2, 'c'), (1, 'd')]
-- a context that only observes the keys cannot distinguish them …
example : (stab recs).map Prod.fst = (unst recs).map Prod.fst := by decide
-- … the context that observes the whole records can
example : (stab recs).map Prod.snd = ['b', 'd', 'a', 'c'] := by decide
example : (unst recs).map Prod.snd = ['d', 'b', 'c', 'a'] := by decide
```
````
