#definition #theorem #example #program

A **lens** from a *source* type `s` to a *focus* type `a` is a pair
```haskell
get :: s -> a
set :: s -> a -> s
```
objectifying read/write access to a part of a larger object (a field of a record, a component of a pair, a column of a database row — where lenses were first introduced). A lens is **lawful** if it satisfies
$$\texttt{set s (get s)} = \texttt{s} \ (\text{set/get}), \qquad \texttt{get (set s a)} = \texttt{a} \ (\text{get/set}), \qquad \texttt{set (set s a) a'} = \texttt{set s a'} \ (\text{set/set}).$$

> Sources: DaoFP §16.3 ("Lenses"), §17.9 ("Existential lens"), §18 ("Tambara Modules", profunctor optics); §16.3 ("Comonad coalgebras").

**Lenses are coalgebras of the [[Store Comonad]].** A coalgebra `phi :: s -> Store a s`, `phi s = St (set s) (get s)`, satisfies $\varepsilon \circ \phi = \mathrm{id}$ iff `set s (get s) = s`, and $W\phi \circ \phi = \delta \circ \phi$ iff `phi . set s = \x -> St (set s) x`, i.e. `set (set s a) = set s` (set/set) and `get (set s a) = a` (get/set). So lawful lenses are exactly the comonad coalgebras — the (co-)[[Eilenberg-Moore Category]] of the store comonad.

Other presentations: the **existential lens** $\int^c (s \to c \times a) \times (c \times b \to t)$ ([[Coend]], DaoFP §17.9), and **profunctor optics** — lenses as maps polymorphic over [[Tambara Module|Tambara modules]] (`type Lens s t a b = forall p. Tambara p => p a b -> p s t`).

````tabs
tab: Julia
```julia
# a lens on a NamedTuple field, and its laws checked on an example
struct Lens; get::Function; set::Function; end
fst_lens = Lens(p -> p.x, (p, a) -> (x = a, y = p.y))
p = (x = 1, y = 2)
fst_lens.set(p, fst_lens.get(p)) == p                          # set/get
fst_lens.get(fst_lens.set(p, 9)) == 9                          # get/set
fst_lens.set(fst_lens.set(p, 5), 7) == fst_lens.set(p, 7)      # set/set
```
tab: Lean
```lean
import Mathlib
structure Lens (s a : Type) where
  get : s → a
  set : s → a → s
  set_get : ∀ x, set x (get x) = x
  get_set : ∀ x v, get (set x v) = v
  set_set : ∀ x v w, set (set x v) w = set x w
def fstLens : Lens (α × β) α := ⟨Prod.fst, fun p a => (a, p.2), by simp, by simp, by simp⟩
```
tab: Haskell
```haskell
data Lens s a = Lens { get :: s -> a, set :: s -> a -> s }
fstL :: Lens (a, b) a
fstL = Lens fst (\(_, b) a -> (a, b))

-- the store-comonad coalgebra of a lens
phi :: Lens s a -> s -> Store a s
phi (Lens g st) s = St (st s) (g s)
```
````
