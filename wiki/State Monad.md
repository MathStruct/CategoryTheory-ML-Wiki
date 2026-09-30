#example #definition #program #theorem

The **state monad** makes state manipulation explicit: a stateful $a \to b$ becomes $(a, s) \to (b, s)$, curried to `a -> State s b` with `newtype State s a = State (s -> (a, s))`.
```haskell
instance Monad (State s) where
  st >>= k = State (\s -> let (a, s') = runState st s in runState (k a) s')
  return a = State (\s -> (a, s))
join mma = State (uncurry runState . runState mma)
get :: State s s          ; get = State (\s -> (s, s))
set :: s -> State s ()    ; set s = State (\_ -> ((), s))
```
`get` and `set` are the basic [[Kleisli Category|Kleisli arrows]] from which every stateful computation is built.

> Sources: DaoFP §14.1 ("State"), §14.4, §15.3 ("The currying adjunction and the state monad"), §15.4 ("State monad transformer"), §14.8 (the stack calculator interpreter is a state functor); §16 (the dual costate comonad, [[Lens]]); CTfS Examples 5.3.3.8–5.3.3.9

**Context in science** (CTfS Example 5.3.3.9): with $s$ the set of *scientific models*, a Kleisli arrow $a \to (b \times s)^s$, i.e. $a \times s \to b \times s$, is a step of reasoning performed in the presence of a model that may also update it — the monad CTfS uses to make sense of its cover diagram of the scientific method ([[Monad]]). With a time preorder on $s$ and outputs required to be later than inputs one gets the "time" monad of CTfS Example 5.3.3.8.

**From the currying adjunction** ([[Monads from Adjunctions]]): $L_s a = a \times s$ is left adjoint to $R_s c = c^s$ ([[Currying]]), and $R_s L_s a = (a \times s)^s$ is `State s a`. The unit $\eta_a : a \to (a \times s)^s$, `\a -> \s -> (a, s)`, is `return`; the counit $\varepsilon_c : c^s \times s \to c$ is function application — `uncurry runState`; and $\mu = R_s \varepsilon L_s$ is `fmap counit`, i.e. `join mma = State (fmap (uncurry runState) (runState mma))`. The [[Monad Transformer]] `StateT s m a = s -> m (a, s)` arises by putting another monad between $L_s$ and $R_s$; `State s = StateT s Identity`.

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# State as functions s -> (a, s)
ret(a) = s -> (a, s)
bind(st, k) = s -> ((a, s′) = st(s); k(a)(s′))
get = s -> (s, s)
set(s) = _ -> (nothing, s)
counter = bind(get, n -> bind(set(n + 1), _ -> ret(n)))   # return old value, increment
counter(5)                                                 # (5, 6)
```
tab: Lean
```lean
import Mathlib
#check @StateT               -- StateT σ m α := σ → m (α × σ)
#check @StateT.bind
#check @get
#check @set
example : StateM ℕ ℕ := do let n ← get; set (n + 1); pure n
```
tab: Haskell
```haskell
newtype State s a = State (s -> (a, s))
runState :: State s a -> s -> (a, s)
runState (State h) s = h s
instance Functor (State s) where fmap f (State h) = State (\s -> let (a, s') = h s in (f a, s'))
instance Applicative (State s) where
  pure a = State (\s -> (a, s))
  State f <*> State g = State (\s -> let (h, s') = f s; (a, s'') = g s' in (h a, s''))
instance Monad (State s) where
  st >>= k = State (\s -> let (a, s') = runState st s in runState (k a) s')
get :: State s s
get = State (\s -> (s, s))
set :: s -> State s ()
set s = State (\_ -> ((), s))
```
````
