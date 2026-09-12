#annotation #example #definition

A **total** function is defined on all arguments; a **pure** function uses only its arguments (and captured values). A **side effect** is anything breaking totality or purity — the "shotgun approach" of imperative languages that makes composition of effects a case-by-case affair. Purely functional languages instead *encode effects in return types*: a computation $a \to b$ with effect becomes a pure $a \to f\, b$ for a type constructor $f$; composing such computations is what a [[Monad]] provides ([[Kleisli Category]]).

> Sources: DaoFP §14.1 ("Programming with Side Effects": "Partiality", "Logging", "Environment", "State", "Nondeterminism", "Input/Output", "Continuation"), §14.4 ("Monad Instances").

| effect | pure encoding | functor | monad note |
|---|---|---|---|
| partiality (exceptions) | `a -> Maybe b`, `a -> Either e b` | `Maybe`, `Either e` | [[Maybe Monad]]; the caller *must* handle `Nothing` (unlike null pointers) |
| logging / auditing | `a -> Writer w b` | `newtype Writer w a = Writer (a, w)` | [[Writer Monad]]; "make the function provide all the data, and let the caller deal with the effects" |
| read-only environment | `(a, e) -> b` curried to `a -> Reader e b` | `newtype Reader e a = Reader (e -> a)` | [[Reader Monad]]; a *delayed* effect — a script run by `runReader` |
| mutable state | `(a, s) -> (b, s)` curried to `a -> State s b` | `newtype State s a = State (s -> (a, s))` | [[State Monad]]; `runState` is function application |
| nondeterminism | `a -> [b]` (many-worlds: all results at once) | `[]` | [[List Monad]] |
| input/output | `a -> IO b` | opaque `IO` (no `runIO`) | [[IO Monad]]; the program *generates a script* executed by the runtime; `main :: IO ()` |
| continuation | `a -> Cont r b` | `newtype Cont r a = Cont ((a -> r) -> r)` | [[Continuation Monad]]; covariant since `a` is in doubly negative position; $K_r a = r^{r^a}$ |

At this stage nothing is assumed of $f$ — not even functoriality; that comes with effect *composition*. Imperative languages hard-code some of these (exceptions ≈ `Either`, tasks ≈ continuations, coroutines ≈ [[Do Notation]]).

````tabs
tab: Julia
```julia
# the same encodings in Julia
partial(x) = x == 0 ? nothing : Some(1 / x)              # Maybe
logging(x) = (x^2, "squared $x")                          # Writer: value paired with a log
reader(x) = env -> x + env[:offset]                       # Reader: delayed access to the environment
state(x) = s -> (x + s, s + 1)                            # State: returns value and new state
nondet(x) = [x, -x]                                       # List: all results at once
```
tab: Haskell
```haskell
newtype Writer w a = Writer (a, w)
newtype Reader e a = Reader (e -> a)
runReader :: Reader e a -> e -> a
runReader (Reader h) e = h e
newtype State s a = State (s -> (a, s))
runState :: State s a -> s -> (a, s)
runState (State h) s = h s
newtype Cont r a = Cont ((a -> r) -> r)
runCont :: Cont r a -> (a -> r) -> r
runCont (Cont f) k = f k
main :: IO ()
main = getLine >>= putStrLn          -- the IO script: echo a line
```
````
