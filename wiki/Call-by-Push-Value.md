#definition #theorem #example #program

**Call-by-push-value** (CBPV, Levy) is a λ-calculus that separates **values**, which *are*, from **computations**, which *do*:

$$
\text{value types } A ::= U\underline B \mid 1 \mid A \times A \mid A + A, \qquad
\text{computation types } \underline B ::= F A \mid A \to \underline B \mid \underline B \,\&\, \underline B.
$$

$F A$ is the type of computations that may perform effects and then **return** a value of type $A$; $U\underline B$ is the type of **thunks** — suspended computations, which are values and can be stored, copied and passed. Two pairs of constructs move between the worlds: `return V` and `M to x. N` (run $M$, bind its value, continue) for $F$; `thunk M` and `force V` for $U$. A λ-abstraction is a *computation* that pops an argument; applying it pushes one — hence the name.

The slogan: **a value is, a computation does**. Effects happen only when computations run, so the order and number of effects is explicit in the term.

> Sources: Levy, *Call-By-Push-Value: A Functional/Imperative Synthesis* (Springer, Semantic Structures in Computation 2, 2003); Levy, *Call-by-push-value: a subsuming paradigm*, TLCA 1999; Plotkin & Pretnar [arXiv:1312.1399](https://arxiv.org/abs/1312.1399) ([[Handling Algebraic Effects|notes]]) §2 (their handler calculus extends CBPV, and Levy's equations hold, §5); Levy, Power & Thielecke, *Modelling environments in call-by-value programming languages*, Inform. and Comput. 185 (2003).

## CBV and CBN are both inside

Both classical evaluation orders translate into CBPV, which makes their difference visible. For $(\lambda x.\, x + x)\; M$ with an effectful argument $M$:

- **call-by-value**: `M to v. (λx. x + x) v` — the argument is evaluated once, its *value* is passed;
- **call-by-name**: `(λx. force x + force x) (thunk M)` — a *thunk* is passed, and each use forces it.

Under CBV the effects of $M$ happen once, under CBN twice. A CBV function type $A \to B$ becomes $U(A \to F B)$, a CBN one $U\underline A \to \underline B$. CBPV is the common refinement in which both are definable and both sets of equations hold.

## Models are adjunctions

The categorical semantics of CBPV is an [[Adjunction]] $F \dashv U$ between a category of values and a category of computations (more precisely, of algebras or stacks):

$$
\mathcal V \;\underset{U}{\overset{F}{\rightleftarrows}}\; \mathcal C, \qquad \mathcal C(FA, \underline B) \;\cong\; \mathcal V(A, U\underline B).
$$

The composite $UF$ is a [[Monad]] on values — the monad of the effect, recovered as in [[Monads from Adjunctions]] — and $FU$ a [[Comonad]] on computations. For the state monad, $\mathcal C$ is the category of algebras for it; for a monad $T$ in general, the [[Eilenberg-Moore Category]] $F \dashv U$ is the canonical model. The CBV fragment of a CBPV model is a [[Freyd Category]], so CBPV refines Moggi's monads: it keeps the adjunction the monad came from. [[Algebraic Effects and Handlers|Handlers]] live naturally on the computation side, as maps between algebras.

## Sophia

Sophia's Core Calculus puts effects on the *judgement* ($\Gamma \vdash t : A \,!\, \varepsilon$) rather than only on types, with the rule that building a closure has the empty effect and calling it performs the function's effects — exactly the CBPV distinction between `thunk` (a value, effect-free) and `force`/application (a computation). Its two fragments, a total proof fragment `Prf` and a general computational fragment `Cmp`, are a coarser version of the value/computation split, with a one-way inclusion `Prf ↪ Cmp`. See [Core Calculus](https://mathstruct.org/Sophia/vault/Design/Core-Calculus).

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# Call-by-push-value: values V ::= n | thunk M ; computations M ::= return V | M to x. N | force V | λx. M | M V | print s; M
# Computations run, values are inert. The interpreter threads an output log (the effect).
function run(M, log)
    k = M[1]
    k == :return ? M[2] :
    k == :to     ? run(M[3](run(M[2], log)), log) :          # M to x. N : run M, bind its value, run N
    k == :force  ? run(M[2][2], log) :                        # force (thunk M) = M
    k == :print  ? (push!(log, M[2]); run(M[3], log)) :
    k == :app    ? run(M[2][2](M[3]), log) :                  # (λx. M) V
    error("not a computation")
end
thunk(M) = (:thunk, M); force(V) = (:force, V); ret(V) = (:return, V); to(M, f) = (:to, M, f)
lam(f) = (:lam, f); app(M, V) = (:app, M, V)
plus(M, N) = to(M, a -> to(N, b -> ret(a + b)))
effectful = (:print, "hi", ret(1))                            # print "hi"; return 1
# Call-by-value translation of (λx. x + x) effectful: evaluate the argument first, pass the *value*.
cbv = to(effectful, v -> app(lam(x -> plus(ret(x), ret(x))), v))
# Call-by-name translation: pass a *thunk*; every use of x forces it.
cbn = app(lam(x -> plus(force(x), force(x))), thunk(effectful))
log1 = String[]; (run(cbv, log1), log1)                       # (2, ["hi"]): the effect happens once
log2 = String[]; (run(cbn, log2), log2)                       # (2, ["hi", "hi"]): the effect happens twice
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
-- A CBPV model is an adjunction F ⊣ U; the monad of the effect is U ∘ F.
#check @Adjunction.toMonad      -- (L ⊣ R) → Monad C
#check @Adjunction.toComonad    -- (L ⊣ R) → Comonad D
-- Lean's own `Thunk α` is U(F α): a suspended computation that is a value; `get` forces it.
example : (Thunk.mk fun _ => (3 : Nat)).get = 3 := rfl
```
````
