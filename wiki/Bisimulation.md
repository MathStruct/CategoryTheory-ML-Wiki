#definition #theorem #example #program

A **bisimulation** between two systems is a relation $R$ on their states such that related states make the same observation now and their successors can be related again. For [[Coalgebra of an Endofunctor|coalgebras]] $\alpha : A \to FA$ and $\beta : B \to FB$ of an endofunctor $F$, the categorical definition (Aczel–Mendler) is: $R \subseteq A \times B$ is a bisimulation if it carries a coalgebra structure $\gamma : R \to FR$ making both projections coalgebra morphisms,

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}[column sep=large, row sep=large]
A \arrow[d, "\alpha"'] & R \arrow[l, "\pi_1"'] \arrow[r, "\pi_2"] \arrow[d, "\gamma"] & B \arrow[d, "\beta"] \\
FA & FR \arrow[l, "F\pi_1"] \arrow[r, "F\pi_2"'] & FB
\end{tikzcd}
\end{document}
```

**Bisimilarity** $\sim$ is the union of all bisimulations — itself the greatest bisimulation, and the **greatest fixed point** of a monotone operator on relations ([[Least Fixed Point]], dually). For a labelled transition system ($F X = \mathcal P(L \times X)$) this is Milner's strong bisimulation: $s \sim t$ iff every step $s \xrightarrow{a} s'$ is matched by some $t \xrightarrow{a} t'$ with $s' \sim t'$, and vice versa.

> Sources: Rutten, *Universal coalgebra: a theory of systems*, Theor. Comput. Sci. 249 (2000) (bisimulations of coalgebras, final coalgebras, coinduction); Jacobs, *Introduction to Coalgebra: Towards Mathematics of States and Observation* (CUP 2016), Chapter 3 (bisimulations), Chapter 4 (relation lifting and finality); Sangiorgi, *On the origins of bisimulation and coinduction*, ACM TOPLAS 31(4) (2009); Kanellakis & Smolka, *CCS expressions, finite state processes, and three problems of equivalence*, Inform. and Comput. 86 (1990) and Paige & Tarjan, *Three partition refinement algorithms*, SIAM J. Comput. 16 (1987) (computing bisimilarity by partition refinement). See [[Terminal Coalgebra]] and [[Finite State Machine]] for the coalgebraic background.

## Bisimilarity is equality in the terminal coalgebra

Every coalgebra has a unique morphism $!_A : A \to \nu F$ into the [[Terminal Coalgebra]] — "the behaviour of a state". For functors that preserve weak pullbacks (all polynomial functors and $\mathcal P$ do),

$$
a \sim b \quad\Longleftrightarrow\quad !_A(a) = !_B(b):
$$

two states are bisimilar iff they have the same behaviour. This is the **coinduction principle**: to prove two infinite objects equal, exhibit a bisimulation. For term graphs ($F X = \mathrm{Tag} \times X^*$) the terminal coalgebra is the set of possibly infinite trees, and two nodes are bisimilar iff they have the **same infinite unfolding**. Hash-consing a cyclic structure *correctly* means hashing its image in $\nu F$.

## Computing it: partition refinement

On a finite system, bisimilarity is computed from the top: start with the partition by observation (tags), and repeatedly split blocks whose members have successors in different blocks, until stable. This is the greatest-fixed-point iteration; Kanellakis–Smolka do it naively, Paige–Tarjan in $O(m \log n)$ by "process the smaller half". The same algorithm is called **colour refinement** or **1-dimensional Weisfeiler–Leman** in graph isomorphism testing and graph neural networks. The quotient by bisimilarity is the **minimal** system with the same behaviour — DFA minimisation is the special case $F X = 2 \times X^\Sigma$.

## Variants used for real languages

- **Weak bisimulation** ignores internal (τ) steps, so an implementation may take more steps than its specification — "same behaviour, different performance".
- **Applicative** and **environmental** bisimulations adapt the idea to higher-order languages, where a step must account for functions being passed to unknown contexts.
- **Bisimulation up to** (up to context, up to equivalence) lets one exhibit a much smaller relation and close it under a sound function; it is what makes the method practical, and it fits a "store the witness, replay the check" architecture: the relation is the evidence, the matching conditions are the checker.

## Bisimulation and logical relations

Bisimulations are the coinductive, *state-based* way to prove two programs equivalent; [[Logical Relations]] are the inductive, *type-based* way. For effectful and higher-order languages one uses both: applicative and environmental bisimulations for languages with state, logical relations for typed λ-calculi. Both exist to prove that an equivalence is a [[Congruence]] without quantifying over all contexts ([[Contextual Equivalence]]).

## Sophia

Sophia hashes a strongly connected component of mutually recursive definitions by colour refinement, then breaks any remaining ties by a canonical ordering. Coalgebraically, refinement computes bisimilarity, so members that refinement cannot separate are **bisimilar** — they have identical unfoldings, as `even`/`odd` do in the Julia tab. Such members could be given the *same* hash (their image in the terminal coalgebra) rather than distinguished by a tie-break; distinguishing them is a design choice about whether a definition's identity includes its *name within the cycle*. See [Hashing and Identity](https://mathstruct.org/Sophia/vault/Design/Hashing-and-Identity) and [Equivalence and Witnesses](https://mathstruct.org/Sophia/vault/Design/Equivalence-and-Witnesses).

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# A coalgebra for F(X) = Tag × X*: each node has a tag and an ordered list of children (a term graph, cycles allowed).
# Two mutually recursive pairs with the same shape:  even/odd  and  ev/od, plus a different pair p/q.
tag   = Dict(:even => :if, :odd => :if, :ev => :if, :od => :if, :p => :if, :q => :call)
child = Dict(:even => [:odd], :odd => [:even], :ev => [:od], :od => [:ev], :p => [:q], :q => [:p])
nodes = collect(keys(tag))
# Partition refinement (1-dimensional Weisfeiler–Leman / Kanellakis–Smolka): start from the tag,
# refine by the colours of the children until stable. The fixed point is bisimilarity.
function refine(nodes, tag, child)
    col = Dict(n => hash(tag[n]) for n in nodes)
    while true
        new = Dict(n => hash((col[n], [col[c] for c in child[n]])) for n in nodes)
        length(unique(values(new))) == length(unique(values(col))) && return new
        col = new
    end
end
col = refine(nodes, tag, child)
col[:even] == col[:ev] && col[:odd] == col[:od]      # true: the two pairs are bisimilar
col[:even] == col[:odd]                              # true as well: even and odd have the same unfolding!
col[:p] == col[:even]                                # false: q has a different tag
# bisimilar ⇔ same (infinite) unfolding: compare unfoldings to depth d
unfold(n, d) = d == 0 ? tag[n] : (tag[n], [unfold(c, d - 1) for c in child[n]])
unfold(:even, 6) == unfold(:od, 6)                   # true
unfold(:p, 6) == unfold(:even, 6)                    # false
```
tab: Lean
```lean
import Mathlib
-- A bisimulation for a stream-like coalgebra next : S → ℕ × S (observe a number, step to a successor).
def IsBisim {S : Type*} (next : S → ℕ × S) (R : S → S → Prop) : Prop :=
  ∀ s t, R s t → (next s).1 = (next t).1 ∧ R (next s).2 (next t).2

-- bisimilar states produce the same observations forever (coinduction, unfolded to any depth)
theorem bisim_obs {S : Type*} (next : S → ℕ × S) (R : S → S → Prop) (h : IsBisim next R) :
    ∀ n s t, R s t → (next ((fun x => (next x).2)^[n] s)).1 = (next ((fun x => (next x).2)^[n] t)).1 := by
  intro n
  induction n with
  | zero => intro s t hst; exact (h s t hst).1
  | succ n ih =>
    intro s t hst
    simp only [Function.iterate_succ, Function.comp]
    exact ih _ _ (h s t hst).2
```
tab: Haskell
```haskell
-- Two cyclic structures with the same infinite unfolding are bisimilar; laziness lets us compare prefixes.
data Tree = Node String [Tree]

prefix :: Int -> Tree -> String
prefix 0 _ = "."
prefix d (Node t cs) = t ++ "(" ++ concatMap (prefix (d - 1)) cs ++ ")"

evenT, oddT, evT, odT :: Tree
evenT = Node "if" [oddT]
oddT  = Node "if" [evenT]
evT   = Node "if" [odT]
odT   = Node "if" [evT]

main :: IO ()
main = print (prefix 8 evenT == prefix 8 odT)   -- True: same unfolding to depth 8
```
````
