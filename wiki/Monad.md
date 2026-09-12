#definition #theorem #example #program

A **monad** on a category $\mathcal{C}$ is a [[Monoid Object|monoid]] in the strict [[Monoidal Category|monoidal category]] $([\mathcal{C}, \mathcal{C}], \circ, \mathrm{Id})$ of [[Endofunctor|endofunctors]] under composition (horizontal composition of natural transformations is the tensor of arrows): a triple $(T, \eta, \mu)$ of an endofunctor $T$ and [[Natural Transformation|natural transformations]]
$$\eta : \mathrm{Id} \to T \ (\text{unit}, \texttt{return}), \qquad \mu : T \circ T \to T \ (\text{multiplication}, \texttt{join}),$$
satisfying the unit laws $\mu \circ (\eta \circ T) = \mathrm{id}_T = \mu \circ (T \circ \eta)$ and associativity $\mu \circ (\mu \circ T) = \mu \circ (T \circ \mu)$ (whiskering notation). Monads were once called "triples", hence $T$.

> Sources: DaoFP Chapter 14 ("Monads": §14.1 "Programming with Side Effects", §14.2 "Composing Effects", §14.3 "Alternative Definitions", §14.4 "Monad Instances", §14.5 "Do Notation", §14.7 "Monads Categorically": "Substitution", "Monad as a monoid"), Chapter 15 (monads from adjunctions, monad algebras); 7 Sketches §1.4.4 ([[Closure Operator|closure operators]] are monads on preorders), Example 1.123; Kittenlab Lecture 7 (the free-monoid round trip).

## Three equivalent definitions (§14.2–14.3)

Programming motivation: replace an effectful computation $a \to b$ by a pure function $a \to f\, b$ where $f$ encodes the effect ([[Side Effects as Functors]]). To decompose one big such step into small ones we need to *compose* $g : a \to f b$ and $h : b \to f c$.

| definition | data | laws |
|---|---|---|
| **Kleisli** ("fish") | `(<=<) :: (b -> m c) -> (a -> m b) -> (a -> m c)`, `return :: a -> m a` | associativity and unit laws of the [[Kleisli Category]] |
| **join** (mathematicians') | `join :: m (m a) -> m a`, `return` | monoid laws above |
| **bind** (programmers') | `(>>=) :: m a -> (a -> m b) -> m b`, `return` | `return a >>= k = k a`, `m >>= return = m`, associativity |

Translations: `g <=< f = join . fmap g . f`; `join = id <=< id`; `g <=< f = \a -> f a >>= g`; `ma >>= k = (k <=< id) ma`; `join mma = mma >>= id`; a bind-monad is automatically a functor via `liftM f ma = ma >>= (return . f)`. The fish operator is point-free programming; bind names the intermediate value, which [[Do Notation]] makes pleasant. "The sole purpose of monads in programming is to let us decompose one big Kleisli arrow into multiple smaller ones."

## Instances

[[Maybe Monad]] (partiality), [[Writer Monad]] (logging), [[Reader Monad]] (environment), [[State Monad]], [[List Monad]] (nondeterminism), [[Continuation Monad]], [[IO Monad]]; the expression monad `Ex` whose bind is **substitution** of expressions for variables (`ex >>= sub` replaces $a \mapsto x_1 + 2$, $b \mapsto x_2$) — the algebraic origin of monads; [[Free Monad|free monads]]; on a preorder, a monad is a [[Closure Operator]].

## Structure

- Every [[Adjunction]] $L \dashv R$ gives a monad $(R L, \eta, R \varepsilon L)$ ([[Monads from Adjunctions]]); every monad arises this way, from its [[Eilenberg-Moore Category]] (terminal such adjunction) or [[Kleisli Category]] (initial). Monads do not compose in general, but adjunctions do — [[Monad Transformer|monad transformers]].
- The category of monads $\mathbf{Mon}(\mathcal{C})$ has as morphisms natural transformations $\lambda : T \to T'$ preserving $\eta$ and $\mu$ — monoid homomorphisms in $[\mathcal{C}, \mathcal{C}]$; the forgetful functor to $[\mathcal{C}, \mathcal{C}]$ has a left adjoint only sometimes ([[Free Monad]]).
- [[String Diagram|String diagrams]] draw $\eta$ as a dot spawning a $T$-string and $\mu$ as two strings merging; the laws are "yanking" pictures.
- In a [[Cartesian Closed Category]] every monad has [[Functorial Strength|strength]] and is hence an [[Applicative Functor]]; monads are more powerful than applicatives because monadic code can branch on results.
- Dual: [[Comonad]].

````tabs
tab: Julia
```julia
# a monad as (fmap, return, join) on a type constructor: the Maybe monad via Union{Some, Nothing}
fmap(f, ::Nothing) = nothing
fmap(f, x::Some) = Some(f(something(x)))
ret(a) = Some(a)
join(::Nothing) = nothing
join(x::Some) = something(x)                    # Some(Some(a)) ↦ Some(a), Some(nothing) ↦ nothing
bind(ma, k) = join(fmap(k, ma))
fish(g, f) = a -> bind(f(a), g)                 # Kleisli composition g <=< f
safediv(x) = y -> y == 0 ? nothing : Some(x / y)
bind(Some(2.0), safediv(8.0))                   # Some(4.0)
bind(Some(0.0), safediv(8.0))                   # nothing
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
#check @CategoryTheory.Monad                 -- structure on C ⥤ C with η, μ and the three laws
#check @CategoryTheory.Monad.assoc
#check @CategoryTheory.Monad.left_unit
#check @CategoryTheory.Adjunction.toMonad    -- every adjunction gives a monad
#check @Monad                                -- Lean's programming monad class (bind, pure)
example : Option ℕ := do let x ← some 2; let y ← some 3; pure (x + y)   -- do notation
```
tab: Haskell
```haskell
class Functor m => Monad' m where              -- the Kleisli definition (DaoFP §14.2)
  (<=<)   :: (b -> m c) -> (a -> m b) -> (a -> m c)
  return' :: a -> m a

class Functor m => MonadJ m where              -- the join definition
  join    :: m (m a) -> m a
  returnJ :: a -> m a

-- Prelude: class Applicative m => Monad m where (>>=) :: m a -> (a -> m b) -> m b; return :: a -> m a
-- translations
kleisli :: Monad m => (b -> m c) -> (a -> m b) -> (a -> m c)
kleisli g f = \a -> f a >>= g
joinM :: Monad m => m (m a) -> m a
joinM mma = mma >>= id
liftM :: Monad m => (a -> b) -> m a -> m b
liftM f ma = ma >>= (return . f)

-- substitution monad: bind substitutes expressions for variables
data Ex x = Val Int | Var x | Plus (Ex x) (Ex x) deriving (Functor, Show)
instance Applicative Ex where pure = Var; (<*>) = liftM2 id
instance Monad Ex where
  Val n >>= _ = Val n
  Var x >>= k = k x
  Plus e1 e2 >>= k = Plus (e1 >>= k) (e2 >>= k)
```
````
