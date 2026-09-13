#definition #theorem #example #program

Monads do not compose in general, but [[Adjunction|adjunctions]] do. Given an "inner" monad $T = R' L'$ and an "outer" adjunction $L \dashv R$, the composite $R \circ T \circ L$ is again a monad ([[Monads from Adjunctions]]) — the **monad transformer** of the outer adjunction applied to $T$. Its unit and multiplication, drawn as [[String Diagram|string diagrams]], are

$$
\eta_a = R(\eta^i_{L a}) \circ \eta^o_a, \qquad \mu_c = R(\mu^i_{L c}) \circ (R T)(\varepsilon^o_{(T L) c}).
$$

> Sources: DaoFP §15.4 ("Monad Transformers", "State monad transformer"); the Monad Transformer Library (MTL).

**State transformer.** With the currying adjunction $L_s a = a \times s$, $R_s c = c^s$: `newtype StateT s m a = StateT (s -> m (a, s))`, `return x = StateT (\s -> return (x, s))` (outer unit followed by the inner `return` post-composed via $R$), and
```haskell
join mma = StateT (join . fmap (uncurry runStateT) . runStateT mma)
```
(outer counit = `uncurry runStateT` lifted by $R T$ = post-composition and `fmap`, then the inner `join`). Then `MaybeState s a = StateT s Maybe a` is `s -> Maybe (a, s)`, combining state with failure — matching the hand-written instance whose laws one would otherwise have to check by hand — and `State s = StateT s Identity`.

````tabs
tab: Julia
```julia
# StateT over Maybe: s -> Union{Some{(a, s)}, Nothing}
retST(a) = s -> Some((a, s))
bindST(st, k) = s -> (r = st(s); r === nothing ? nothing : ((a, s′) = something(r); k(a)(s′)))
popST = s -> isempty(s) ? nothing : Some((s[1], s[2:end]))          # fails on empty stack
prog = bindST(popST, x -> bindST(popST, y -> retST(x + y)))
prog([3, 4, 5])                     # Some((7, [5]))
prog([3])                           # nothing
```
tab: Lean
```lean
import Mathlib
#check @StateT               -- StateT σ m α := σ → m (α × σ)
#check @OptionT
#check @ReaderT
example : StateT (List ℕ) Option ℕ := do
  let s ← get
  match s with
  | x :: rest => set rest; pure x
  | [] => failure
```
tab: Haskell
```haskell
newtype StateT s m a = StateT (s -> m (a, s))
runStateT :: StateT s m a -> s -> m (a, s)
runStateT (StateT h) = h
instance Functor m => Functor (StateT s m) where
  fmap f (StateT h) = StateT (fmap (\(a, s) -> (f a, s)) . h)
instance Monad m => Applicative (StateT s m) where
  pure x = StateT (\s -> return (x, s))
  mf <*> ma = mf >>= \f -> fmap f ma
instance Monad m => Monad (StateT s m) where
  mma >>= k = StateT (\s -> runStateT mma s >>= \(a, s') -> runStateT (k a) s')
type MaybeState s a = StateT s Maybe a
```
````
