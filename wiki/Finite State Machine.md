#definition #example #theorem #proof #program

A (deterministic) **finite state machine** is a quintuple $(\Sigma, S, s_0, \delta, F)$: a finite nonempty **input alphabet** $\Sigma$, a finite nonempty **state set** $S$, a **state-transition function** $\delta : \Sigma \times S \to S$, an initial state $s_0 \in S$ and a set $F \subseteq S$ of final states. Category Theory for Scientists ignores $s_0$ and $F$ and focuses on how the alphabet acts on the states:

> **Slogan (CTfS 3.1.2.12).** *A finite state machine is an action of a free monoid on a finite set.*

**Proposition (CTfS 3.1.2.11).** For finite nonempty $\Sigma, S$, giving a function $\delta : \Sigma \times S \to S$ is equivalent to giving an action of the [[Free Monoid]] $\mathrm{List}(\Sigma)$ on $S$ ([[Monoid Action]]).

*Proof.* An action $\circlearrowleft$ of $\mathrm{List}(\Sigma)$ restricts to $\delta(\sigma, s) := [\sigma] \circlearrowleft s$ on one-letter words. Conversely, given $\delta$, define the action of a word by recursion: $[\,] \circlearrowleft s := s$ and $[\sigma_1, \dots, \sigma_n] \circlearrowleft s := [\sigma_1, \dots, \sigma_{n-1}] \circlearrowleft \delta(\sigma_n, s)$. This satisfies the two action laws, and the two constructions are mutually inverse ([[CTfS Chapter 3 Exercises#Exercise 3.1.2.13|CTfS Exercise 3.1.2.13]]). Conceptually this is the universal property of the free monoid, $\mathbf{Mon}(\mathrm{List}(\Sigma), \mathrm{End}(S)) \cong \mathbf{Set}(\Sigma, \mathrm{End}(S))$, combined with [[Currying]]. $\blacksquare$

(With this recursion the *last* letter acts first, so strictly speaking one gets a right action — CTfS's footnote 5; reading words left to right gives the left action of the opposite monoid.)

> Sources: CTfS §3.1.2.10 (Figure 3.1, Proposition 3.1.2.11, Slogan 3.1.2.12, Exercise 3.1.2.13), Example 3.1.3.1, Exercise 3.1.4.15, Application 4.3.1.2, Example 4.3.2.15, Exercises 4.3.2.13, 4.6.2.5; DaoFP Chapter 13 (state machines as [[Coalgebra of an Endofunctor|coalgebras]] $S \to O \times S^\Sigma$).

## The example of CTfS Figure 3.1

Alphabet $\Sigma = \{a, b\}$, states $\{0, 1, 2\}$ (initial state $0$, final states $\{2\}$), with **action table** (CTfS Example 3.1.3.1):

| state | $a$ | $b$ |
|---|---|---|
| 0 | 1 | 2 |
| 1 | 2 | 1 |
| 2 | 0 | 0 |

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}[column sep=large, row sep=large]
0 \arrow[r, "a", bend left] \arrow[dr, "b"', bend right] & 1 \arrow[d, "a"] \arrow[loop right, "b"] \\
 & 2 \arrow[ul, "{a,\, b}"', bend left=10]
\end{tikzcd}
\end{document}
```

The drawn state diagram is the generating graph of the [[Category of Elements]] of the action ([[CTfS Chapter 4 Exercises#Exercise 4.6.2.5|CTfS Exercise 4.6.2.5]]). Restricting scalars along $\mathbb N \to \mathrm{List}(\{a, b\})$, $1 \mapsto [a, b, b]$, gives a one-button machine: $0 \mapsto 1$, $1 \mapsto 2$, $2 \mapsto 0$ (the same in either reading order of the word) ([[CTfS Chapter 3 Exercises#Exercise 3.1.4.15|CTfS Exercise 3.1.4.15]]).

## Morphisms: refining a model

A morphism of state machines on the same alphabet is an equivariant map, i.e. a [[Natural Transformation]] between the functors $\mathrm{List}(\Sigma) \to \mathbf{Set}$. CTfS Application 4.3.1.2: a collaborator proposes a 6-state machine $Y$ (states $0, 1A, 1B, 1C, 2A, 2B$) that is "compatible" with the 3-state $X$ above; the compatibility is exactly a natural transformation $Y \Rightarrow X$ collapsing the letters. Only the squares for the *generators* $a, b$ need checking, since longer words follow by pasting squares. Natural *isomorphisms* are relabelings of states ([[CTfS Chapter 4 Exercises#Exercise 4.3.2.13|CTfS Exercise 4.3.2.13]]). Adding a button for a frequently used sequence is precomposition with a monoid homomorphism $\mathrm{List}(\{m, n, p\}) \to \mathrm{List}(\{a, b\})$, and the refinement survives by *whiskering* (CTfS Example 4.3.2.15).

## Variations

- *Several kinds of state*, each with its own available inputs: functors from a [[Free Category]] (a graph of states and inputs) to $\mathbf{Set}$ — "commands available in one application have no meaning in another" (CTfS Remark 3.1.2.7).
- *Nondeterministic* or *probabilistic* machines: Kleisli actions for the [[Power Set Monad]] or [[Distribution Monad]] ([[Markov Chain]], [[Kleisli Instance]]).
- *Machines with output* (Mealy/Moore) and infinite behaviour: [[Coalgebra of an Endofunctor|coalgebras]] and [[Terminal Coalgebra|terminal coalgebras]] (DaoFP Chapter 13).

````tabs
tab: Julia
**Docs:** [Categories & functors](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.Categories) · [C-set morphisms](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/categorical_algebra/#Catlab.CategoricalAlgebra.CSets) · [ACSets API](https://algebraicjulia.github.io/ACSets.jl/stable/api/) · [Theories & presentations](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/theories/)
```julia
using Catlab
# CTfS Figure 3.1 / Example 3.1.3.1 as an instance on the schema with one object and two loops
@present SchFSM(FreeSchema) begin
  State::Ob
  (a, b)::Hom(State, State)
end
@acset_type FSM(SchFSM)
X = @acset FSM begin State = 3; a = [2, 3, 1]; b = [3, 2, 1] end   # states 0,1,2 ↦ rows 1,2,3
run(M, word, s) = foldl((s, σ) -> M[s, σ], word; init = s)          # the action of List(Σ)
run(X, [:a, :b, :b], 1)                                             # 2, i.e. State 1
# the refined 6-state model Y of Application 4.3.1.2 and the natural transformation Y ⇒ X
Y = @acset FSM begin
  State = 6                     # 0, 1A, 1B, 1C, 2A, 2B
  a = [2, 5, 6, 6, 1, 1]
  b = [5, 3, 4, 3, 1, 1]
end
α = homomorphism(Y, X; initial = (State = [1, 2, 2, 2, 3, 3],))
is_natural(α)                                                       # true: Y refines X
```
tab: Lean
```lean
import Mathlib
#check @DFA                    -- structure: step : σ → α → σ, start, accept
#check @DFA.evalFrom           -- the action of a word (a `List α`) on a state
#check @DFA.evalFrom_append    -- evalFrom s (x ++ y) = evalFrom (evalFrom s x) y : the action law
#check @FreeMonoid.lift        -- Hom(FreeMonoid α, M) ≃ (α → M): the proof of Proposition 3.1.2.11
```
tab: Haskell
```haskell
data Sym = A | B deriving (Eq, Show)
data St  = S0 | S1 | S2 deriving (Eq, Show)

delta :: Sym -> St -> St          -- the action table of CTfS Example 3.1.3.1
delta A S0 = S1; delta B S0 = S2
delta A S1 = S2; delta B S1 = S1
delta A S2 = S0; delta B S2 = S0

run :: [Sym] -> St -> St          -- the induced action of the free monoid [Sym]
run w s = foldl (flip delta) s w  -- run [] = id,  run (u ++ v) = run v . run u
```
````
