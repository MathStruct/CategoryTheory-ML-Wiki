#definition #theorem #example #program

A **lens** from a *source* type `s` to a *focus* type `a` is a pair
```haskell
get :: s -> a
set :: s -> a -> s
```
objectifying read/write access to a part of a larger object (a field of a record, a component of a pair, a column of a database row — where lenses were first introduced). A lens is **lawful** if it satisfies

$$
\texttt{set s (get s)} = \texttt{s} \ (\text{set/get}), \qquad \texttt{get (set s a)} = \texttt{a} \ (\text{get/set}), \qquad \texttt{set (set s a) a'} = \texttt{set s a'} \ (\text{set/set}).
$$

> Sources: DaoFP §16.3 ("Lenses"), §17.9 ("Existential lens"), §18 ("Tambara Modules", profunctor optics); §16.3 ("Comonad coalgebras"); Cruttwell et al. [arXiv:2103.01931](https://arxiv.org/abs/2103.01931) ([[Categorical Foundations of Gradient-Based Learning|notes]]) Definition 2.4; Spivak [arXiv:1908.02202](https://arxiv.org/abs/1908.02202) ([[Generalized Lens Categories via Functors from Cop to Cat|notes]]) (generalized lenses).

**Lenses are coalgebras of the [[Store Comonad]].** A coalgebra `phi :: s -> Store a s`, `phi s = St (set s) (get s)`, satisfies $\varepsilon \circ \phi = \mathrm{id}$ iff `set s (get s) = s`, and $W\phi \circ \phi = \delta \circ \phi$ iff `phi . set s = \x -> St (set s) x`, i.e. `set (set s a) = set s` (set/set) and `get (set s a) = a` (get/set). So lawful lenses are exactly the comonad coalgebras — the (co-)[[Eilenberg-Moore Category]] of the store comonad.

Other presentations: the **existential lens** $\int^c (s \to c \times a) \times (c \times b \to t)$ ([[Coend]], DaoFP §17.9), and **profunctor optics** — lenses as maps polymorphic over [[Tambara Module|Tambara modules]] (`type Lens s t a b = forall p. Tambara p => p a b -> p s t`). All of these are special cases of [[Optic|optics]].

## The category of lenses $\mathbf{Lens}(\mathcal C)$

For a [[Cartesian Category]] $\mathcal C$, lenses assemble into a category (Cruttwell et al. [arXiv:2103.01931](https://arxiv.org/abs/2103.01931) ([[Categorical Foundations of Gradient-Based Learning|notes]]), Definition 2.4):

- **objects**: pairs $(A, A')$ — think $A$ = values, $A'$ = changes (or requests, or corrections) to values. When $A' \ne A$ these are *bimorphic* lenses, the `s t a b` of Haskell's lens libraries;
- **morphisms** $(A, A') \to (B, B')$: pairs $(f, f^\sharp)$ with $f : A \to B$ (the **get**, forward) and $f^\sharp : A \times B' \to A'$ (the **put**, backward);
- **identity**: $(1_A, \pi_2)$;
- **composition**: forward $f \mathbin{;} g$, backward, pointwise,
$$
(f \mathbin{;} g)^\sharp(a, c') \;=\; f^\sharp\bigl(a,\ g^\sharp(f(a),\, c')\bigr).
$$

Run the forward pass to get $b = f(a)$, push the incoming change $c'$ back through $g^\sharp$ *at the point $b$*, then through $f^\sharp$ *at the point $a$*. That is the reverse-mode chain rule, and for the right $\mathcal C$ it *is* backpropagation ([[Reverse Derivative Category]]); the fact that $f^\sharp$ needs the forward input $a$ is why backprop caches activations. Formally, $\mathbf{Lens}(\mathcal C)$ is the [[Grothendieck Construction]] of the fibrewise opposite of the simple fibration: the backward pass lives in the fibre over the point the forward pass visited.

$\mathbf{Lens}(\mathcal C)$ is symmetric monoidal, $(A, A') \otimes (B, B') = (A \times B, A' \times B')$, but **not cartesian**: a lens into the would-be terminal object $(1, 1)$ needs a put $A \times 1 \to A'$, and there are many — so a backward wire cannot be silently deleted. Lenses into $(1, 1)$ are exactly the **costates** $A \to A'$ that cap off a wire; in gradient-based learning the learning rate is such a cap ([[Gradient-Based Learning with Parametric Lenses]]).

| $\mathcal C$ | a lens $(A, A') \to (B, B')$ is | used for |
|---|---|---|
| $\mathbf{Set}$, $A' = A$ | a get/set pair (database views, records) | functional references |
| $\mathbf{Smooth}$, $A' = A$ | a map and its reverse derivative | backpropagation |
| $\mathbf{Set}$, $A'$ = utilities | a play map and a coplay map returning utilities | [[Open Game|open games]] |
| a [[Markov Category]], state-dependent | a channel and its Bayesian inversion | [[Bayesian Lens|Bayesian lenses]] |

Adding a parameter wire gives [[Parametric Lens|parametric lenses]], $\mathbf{Para}(\mathbf{Lens}(\mathcal C))$.

## In compilers and databases

The view-update problem of databases is a lens problem: a query is `get`, translating view edits back is `put`, and the well-behavedness laws are GetPut and PutGet ([[Relational Lens]]).

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
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
