#definition #theorem #example #program

**Algebraic effects** describe a computational effect by its **operations** and their **equations**, as in universal algebra, instead of by a monad chosen up front. State is the operations $\mathrm{get} : 1 \rightsquigarrow S$ and $\mathrm{put} : S \rightsquigarrow 1$ with equations such as "putting then getting returns what was put"; nondeterminism is a binary $\mathrm{choose}$ that is associative, commutative and idempotent; exceptions are nullary $\mathrm{raise}_e$. An **effect theory** is such a signature with equations — a [[Lawvere Theory]], possibly with parameters — and the monad of the effect is the free-model monad it generates (Plotkin & Power): the [[State Monad]], the finite [[Power Set Monad]], the [[Maybe Monad|exception monad]] all arise this way.

A **handler** (Plotkin & Pretnar) gives each operation an interpretation — a **model** of the signature (Definition 4.2). Because computations are elements of the *free* model (Proposition 4.3: the forgetful functor from models to sets has a left adjoint), there is a unique homomorphism from computations into the handler's model: **handling is a fold**, the catamorphism of the [[Free Monad]] on the signature. One program, many handlers: run a stateful program with state passing, with a log, with a transaction that can roll back.

> Sources: Plotkin & Pretnar, *Handling Algebraic Effects*, Logical Methods in Computer Science 9(4) (2013), [arXiv:1312.1399](https://arxiv.org/abs/1312.1399) ([[Handling Algebraic Effects|notes]]) Definitions 4.2, 4.4, 4.5, 6.1, 6.3, Proposition 4.3, Theorems 6.2, 6.5, 6.6; Plotkin & Power, *Notions of computation determine monads*, FoSSaCS 2002, and *Algebraic operations and generic effects*, Applied Categorical Structures 11 (2003); Moggi, *Notions of computation and monads*, Inform. and Comput. 93 (1991) (the monadic view this refines); Hyland & Power, ENTCS 172 (2007) (Lawvere theories and monads). DaoFP Ch. 14–16 for monads in Haskell, [[Side Effects as Functors]] for the programming motivation.

## Correct handlers are models of the theory

A handler that interprets the operations but **violates the equations** of the effect theory is not a model, and programs that are equal in the theory can behave differently under it. Plotkin–Pretnar call a handler *correct* when its clauses form a model (Definition 4.5). The tracing handler in the Julia tab is the standard counterexample: it records `put 5; get` and `put 5` differently, though the theory of state says they are equal. Correctness is genuinely hard to decide: for simple handlers it is $\Pi_2$-complete (Theorem 6.2), for uniformly simple families $\Sigma_1$-complete (Theorem 6.5), and decidable only when the effect theory itself is (Theorem 6.6).

## Why "algebraic"

An operation $\mathrm{op} : A \rightsquigarrow B$ with continuation $k : B \to TX$ is **algebraic** when it commutes with sequencing: $\mathrm{op}(a, k) \mathbin{>\!\!>\!\!=} f = \mathrm{op}(a, \lambda b.\, k(b) \mathbin{>\!\!>\!\!=} f)$. Get, put, choose and raise are algebraic; the *handler* for exceptions (catch) is not, which is precisely why handlers are a separate construct and not just more operations. The calculus Plotkin–Pretnar give is built on [[Call-by-Push-Value]], whose value/computation split is where handlers naturally live.

## Effects and categories

| view | an effectful program $A \to B$ is |
|---|---|
| Moggi | a morphism $A \to TB$ of the [[Kleisli Category]] of a strong monad |
| Plotkin–Power | the same, with $T$ presented by operations and equations |
| Power–Robinson, Levy | a morphism of a [[Freyd Category]] (premonoidal: no interchange law) |
| graded / effect systems | a morphism $A \to T_\varepsilon B$ of a [[Graded Monad]], $\varepsilon$ an effect row |

Effect rows — "this function may `read(r)`, `alloc(r)`, `throw(E)`" — are the grades of a graded monad, and row polymorphism quantifies over them. Koka, Eff and OCaml 5 implement handlers; Unison's *abilities* are algebraic effects with handlers.

## Sophia

Sophia's Core Calculus types judgements with effect rows, $\Gamma \vdash t : A \,!\, \varepsilon$, and its design notes propose reading cross-language effect questions ("does this Julia mutation mean the same as this C++ pointer write?") as questions about **handlers**: two programs are equivalent with respect to an effect if they agree under every *correct* handler — every model of the effect theory. See [Effects Memory and Resources](https://mathstruct.org/Sophia/vault/Design/Effects-Memory-and-Resources) and [Core Calculus](https://mathstruct.org/Sophia/vault/Design/Core-Calculus).

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# Computations over the signature of state: get : 1 ⇝ S and put : S ⇝ 1. A computation is a tree:
#   (:ret, v)  or  (:op, name, argument, continuation)   — the free model of the signature.
ret(v) = (:ret, v)
get_(k) = (:op, :get, nothing, k)
put_(s, k) = (:op, :put, s, k)
bind(c, f) = c[1] == :ret ? f(c[2]) : (:op, c[2], c[3], x -> bind(c[4](x), f))
# A program: read the counter, add one, write it back, return the old value.
incr = get_(s -> put_(s + 1, _ -> ret(s)))
prog = bind(incr, a -> bind(incr, b -> ret((a, b))))
# A handler interprets each operation; handling = the unique fold out of the free model.
function handle(c, h)
    c[1] == :ret ? h.ret(c[2]) : h.ops[c[2]](c[3], x -> handle(c[4](x), h))
end
# Handler 1: state-passing — a computation becomes a function S → (result, final state).
statepassing = (ret = v -> (s -> (v, s)),
                ops = Dict(:get => (_, k) -> (s -> k(s)(s)), :put => (s′, k) -> (_ -> k(nothing)(s′))))
handle(prog, statepassing)(10)                     # ((10, 11), 12)
# Handler 2: the same program, interpreted as a trace of the operations it performs (state fixed at 0).
tracing = (ret = v -> [], ops = Dict(:get => (_, k) -> [:get; k(0)], :put => (s, k) -> [Symbol("put", s); k(nothing)]))
handle(prog, tracing)                              # [:get, :put1, :get, :put1]
# A handler is correct when it satisfies the equations of the effect theory, e.g. put s; get = put s; return s
lhs = put_(5, _ -> get_(x -> ret(x))); rhs = put_(5, _ -> ret(5))
handle(lhs, statepassing)(0) == handle(rhs, statepassing)(0)   # true: the state-passing handler is a model
handle(lhs, tracing) == handle(rhs, tracing)                   # false: tracing distinguishes them — not a model of state
```
tab: Lean
```lean
import Mathlib
-- The free model of an effect signature: operations o with arities Ar o (Plotkin–Pretnar, Prop. 4.3).
inductive Comp (Op : Type) (Ar : Op → Type) (α : Type) where
  | ret : α → Comp Op Ar α
  | op : (o : Op) → (Ar o → Comp Op Ar α) → Comp Op Ar α

-- handling is the fold: a value clause and one clause per operation (an algebra for the signature)
def Comp.handle {Op : Type} {Ar : Op → Type} {α β : Type}
    (val : α → β) (clause : (o : Op) → (Ar o → β) → β) : Comp Op Ar α → β
  | .ret a => val a
  | .op o k => clause o (fun x => (k x).handle val clause)

-- a one-bit choice effect: `flip` has arity Bool; handle by collecting all results
inductive Choice | flip
def coin : Comp Choice (fun _ => Bool) Nat :=
  .op .flip fun b => .ret (if b then 1 else 0)
example : coin.handle (fun n => [n]) (fun _ k => k true ++ k false) = [1, 0] := rfl
```
tab: Haskell
```haskell
-- Free monad over the state signature, and two handlers (folds).
data Comp s a = Ret a | Get (s -> Comp s a) | Put s (Comp s a)

bindC :: Comp s a -> (a -> Comp s b) -> Comp s b
bindC (Ret a) f   = f a
bindC (Get k) f   = Get (\s -> bindC (k s) f)
bindC (Put s c) f = Put s (bindC c f)

incr :: Comp Int Int
incr = Get (\s -> Put (s + 1) (Ret s))

runState :: Comp s a -> s -> (a, s)          -- the state-passing handler
runState (Ret a) s    = (a, s)
runState (Get k) s    = runState (k s) s
runState (Put s' c) _ = runState c s'

trace :: Comp Int a -> [String]               -- a tracing handler (state fixed at 0)
trace (Ret _)   = []
trace (Get k)   = "get" : trace (k 0)
trace (Put s c) = ("put" ++ show s) : trace c

main :: IO ()
main = do
  let prog = incr `bindC` \a -> incr `bindC` \b -> Ret (a, b)
  print (runState prog 10)   -- ((10,11),12)
  print (trace prog)         -- ["get","put1","get","put1"]
```
````
