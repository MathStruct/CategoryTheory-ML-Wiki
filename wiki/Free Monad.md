#definition #theorem #example #program

The **free monad** on an endofunctor $F$ is the free [[Monoid Object|monoid]] in $([\mathcal{C}, \mathcal{C}], \circ, \mathrm{Id})$, i.e. the free [[Monad]]: the [[Initial Algebra]] of the higher-order "list" functor

$$
\Phi_F G = \mathrm{Id} + F \circ G, \qquad L_F \cong \mathrm{Id} + F \circ L_F
$$

(coproducts of functors taken pointwise). Its structure map $\iota = [\eta, \varphi]$ splits into $\eta : \mathrm{Id} \to L_F$ and $\varphi : F \circ L_F \to L_F$, which become the two constructors
```haskell
data FreeMonad f a where
  Pure :: a -> FreeMonad f a
  Free :: f (FreeMonad f a) -> FreeMonad f a
```
A value is a tree whose nodes are functorfuls of branches and whose leaves hold `a`'s. It separates *what* to do from *how*: a sequence of actions is recorded without committing to an interpreter — like building an AST before compiling, or a [[Free Monoid|list]] before choosing an algebra.

> Sources: DaoFP §14.8 ("Free Monads": "Category of monads", "Free monad", "Free Monad in Haskell", "Stack calculator example"), Exercises 14.8.1–14.8.3; §12 ([[Initial Algebra]]); §15.5 (monad algebras).

- **Why not an adjunction?** The forgetful $U : \mathbf{Mon}(\mathcal{C}) \to [\mathcal{C}, \mathcal{C}]$ (objects: monads; morphisms: natural transformations preserving $\eta, \mu$) has a left adjoint only for some $F$ — "monads tend to blow things up" (size issues) — so free monads are defined as fixed points instead.
- **Monad structure**: `eta = Pure`; `mu (Pure fa) = fa; mu (Free ffa) = Free (fmap mu ffa)`; `Pure a >>= k = k a; Free ffa >>= k = Free (fmap (>>= k) ffa)`. `FreeMonad` is itself a higher-order functor (`hmap` along a natural transformation $F \to G$).
- **Interpretation** by a *monad algebra* in the functor category: a carrier endofunctor $G$ with $\alpha : \mathrm{Id} + F \circ G \to G$, i.e. a pair `type MAlg f g a = (a -> g a, f (g a) -> g a)`; the catamorphism `mcata (l, r) (Pure a) = l a; mcata (l, r) (Free ffa) = r (fmap (mcata (l, r)) ffa)`. The algebra contains no recursion — recursion is encoded once in `mcata`; the same program can be run by different algebras (e.g. a pretty printer with carrier `Const String`, [[DaoFP Exercise 14.8.3]]).
- **Stack calculator EDSL**: commands `data StackF k = Push Int k | Top (Int -> k) | Pop k | Add k` (the parameter `k` is the [[Continuation]]); `liftF fr = Free (fmap Pure fr)` and smart constructors `push n = liftF (Push n ())`, `top = liftF (Top id)`, ...; programs in [[Do Notation]]; interpreter `runAlg = (stop, go)` with carrier the state functor `St ([Int] -> ([Int], k))`; `run prog = runAction (mcata runAlg prog) []`.
- Rose trees are `FreeMonad []`, non-empty binary trees `FreeMonad Bin` ([[DaoFP Exercise 14.8.1]], [[DaoFP Exercise 14.8.2]]); the [[List Monad]] is *not* free (its join is irreversible).

````tabs
tab: Julia
```julia
# a free monad over a command functor: Pure(a) | Free(f(FreeMonad))
abstract type FreeM end
struct Pure <: FreeM; a; end
struct Free <: FreeM; ffa; end          # ffa :: StackF{FreeM}
abstract type StackF{K} end
struct Push{K} <: StackF{K}; n::Int; k::K; end
struct Pop{K} <: StackF{K}; k::K; end
struct Add{K} <: StackF{K}; k::K; end
struct Top{K} <: StackF{K}; ik; end      # ik :: Int -> K
fmapS(f, c::Push) = Push{Any}(c.n, f(c.k)); fmapS(f, c::Pop) = Pop{Any}(f(c.k))
fmapS(f, c::Add) = Add{Any}(f(c.k));        fmapS(f, c::Top) = Top{Any}(n -> f(c.ik(n)))
bind(m::Pure, k) = k(m.a)
bind(m::Free, k) = Free(fmapS(x -> bind(x, k), m.ffa))
liftF(c) = Free(fmapS(Pure, c))
push(n) = liftF(Push{Any}(n, nothing)); pop = liftF(Pop{Any}(nothing)); add = liftF(Add{Any}(nothing)); top = liftF(Top{Any}(identity))
calc = bind(push(3), _ -> bind(push(4), _ -> bind(add, _ -> bind(top, x -> bind(pop, _ -> Pure(x))))))
# interpreter = monad algebra with carrier "stack actions" [Int] -> ([Int], k)
run(m::Pure, st) = (st, m.a)
run(m::Free, st) = runc(m.ffa, st)
runc(c::Push, st) = run(c.k, [c.n; st]); runc(c::Pop, st) = run(c.k, st[2:end])
runc(c::Add, st) = run(c.k, [st[1] + st[2]; st[3:end]]); runc(c::Top, st) = run(c.ik(st[1]), st)
run(calc, Int[])                          # (Int[], 7)
```
tab: Lean
```lean
import Mathlib
-- the free monad as an inductive type over a functor-shaped command type
inductive FreeM (f : Type → Type) (α : Type) where
  | pure : α → FreeM f α
  | free : f (FreeM f α) → FreeM f α
#check @CategoryTheory.Monad            -- the category of monads: Monad C with MonadHom
#check @CategoryTheory.MonadHom
```
tab: Haskell
```haskell
data FreeMonad f a where
  Pure :: a -> FreeMonad f a
  Free :: f (FreeMonad f a) -> FreeMonad f a
instance Functor f => Functor (FreeMonad f) where
  fmap g (Pure a) = Pure (g a)
  fmap g (Free ffa) = Free (fmap (fmap g) ffa)
instance Functor f => Applicative (FreeMonad f) where
  pure = Pure
  ff <*> fa = ff >>= \f -> fmap f fa
instance Functor f => Monad (FreeMonad f) where
  Pure a >>= k = k a
  Free ffa >>= k = Free (fmap (>>= k) ffa)

type MAlg f g a = (a -> g a, f (g a) -> g a)
mcata :: Functor f => MAlg f g a -> FreeMonad f a -> g a
mcata (l, _) (Pure a) = l a
mcata (l, r) (Free ffa) = r (fmap (mcata (l, r)) ffa)

data StackF k = Push Int k | Top (Int -> k) | Pop k | Add k deriving Functor
type FreeStack = FreeMonad StackF
liftF :: Functor f => f r -> FreeMonad f r
liftF fr = Free (fmap Pure fr)
push :: Int -> FreeStack (); push n = liftF (Push n ())
pop :: FreeStack ();          pop = liftF (Pop ())
top :: FreeStack Int;         top = liftF (Top id)
add :: FreeStack ();          add = liftF (Add ())
calc :: FreeStack Int
calc = do { push 3; push 4; add; x <- top; pop; return x }

newtype StackAction k = St ([Int] -> ([Int], k)) deriving Functor
runAction :: StackAction k -> [Int] -> ([Int], k)
runAction (St act) = act
runAlg :: MAlg StackF StackAction a
runAlg = (stop, go) where
  stop a = St (\xs -> (xs, a))
  go (Pop k)    = St (\ns -> runAction k (tail ns))
  go (Top ik)   = St (\ns -> runAction (ik (head ns)) ns)
  go (Push n k) = St (\ns -> runAction k (n : ns))
  go (Add k)    = St (\ns -> runAction k ((head ns + head (tail ns)) : tail (tail ns)))
run :: FreeMonad StackF k -> ([Int], k)
run prog = runAction (mcata runAlg prog) []      -- run calc == ([], 7)
```
````
