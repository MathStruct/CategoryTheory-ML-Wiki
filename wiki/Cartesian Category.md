#definition #example #program

A **cartesian category** is a [[Category]] with all binary [[Product|products]] and a [[Terminal Object]] $1$. A *product type* $a \times b$ is defined by its elimination rule — the projections $\mathsf{fst} : a \times b \to a$, $\mathsf{snd} : a \times b \to b$ — and its introduction rule: a mapping *in* $h : c \to a \times b$ is the same as a pair $f : c \to a$, $g : c \to b$. In Haskell, `(a, b)` with the pairing `(x, y)` and pattern matching; records name the projections: `data Product a b = Pair { fst :: a, snd :: b }`.

> Sources: DaoFP Chapter 5 ("Product Types", §5.1 "Cartesian Category", "Tuple Arithmetic", "Functoriality", §5.2 "Duality", §5.3 "Monoidal Category"), §6.1 (`&&&`, `fork`, `bimap`); 7 Sketches Definition 3.86; Kittenlab Lecture 13.

## Tuple arithmetic

All laws are proved "by the mapping-in property" (the Yoneda trick, dual to that for [[Sum Type|sums]]):
- $a \times b \cong b \times a$ — `swap (x, y) = (y, x)`, its own inverse; arrows into $a \times b$ and into $b \times a$ are both determined by the same pair $(f, g)$, and the bijection is natural under pre-composition with $k : x' \to x$.
- $1 \times a \cong a$ — the **left unitor** $\lambda = \mathsf{snd}$ with inverse $\lambda^{-1} = \langle !, \mathrm{id} \rangle$; $\lambda^{-1} \circ \mathsf{snd} = \mathrm{id}$ follows from uniqueness of the mediating arrow ([[DaoFP Exercise 5.1.1]]). Dually the **right unitor** $\rho : a \times 1 \to a$ (`runit`).
- $(a \times b) \times c \cong a \times (b \times c)$ — the **associator** $\alpha$ (`assoc`); the empty tuple `()` is the product of zero types, the unit $1$.
- **Functoriality**: for $f : a \to a'$, $g : b \to b'$, $f \times g := \langle f \circ \mathsf{fst},\ g \circ \mathsf{snd} \rangle : a \times b \to a' \times b'$ (`bimap` of the [[Bifunctor]] `(,)`).
- Symmetry holds only up to isomorphism: swapping components changes how the information is *accessed*, not its content.

Hence $(\mathcal{C}, \times, 1)$ is a [[Symmetric Monoidal Category]]; a category may carry several monoidal structures at once, e.g. both $+$ and $\times$. A cartesian category with [[Exponential Object|exponentials]] is a [[Cartesian Closed Category]]; with sums as well, [[Bicartesian Closed Category|bicartesian closed]]. Mixed maps such as $b + a \times b \to (1 + a) \times b$ are built by decomposing into sums-out and products-in ([[DaoFP Exercise 5.1.2]]–[[DaoFP Exercise 5.1.4]]).

## Duality

"A product is the sum with arrows reversed": rename $\mathsf{Left}, \mathsf{Right}$ to $\mathsf{fst}, \mathsf{snd}$ in the sum diagram and read all arrows backwards. Every construction has a dual ([[Opposite Category]]). What makes sums and products *feel* different in programming is the asymmetry between $0$ (no incoming arrows) and $1$ (many outgoing arrows = [[Global Element|elements]]). In logic a product is conjunction: to prove $A \times B$ provide proofs of both; $\mathsf{fst}, \mathsf{snd}$ extract them. A [[Monoid Object]] is defined inside any cartesian (indeed monoidal) category by $\mu : m \times m \to m$, $\eta : 1 \to m$.

````tabs
tab: Julia
```julia
using Catlab
A = FinSet(2); B = FinSet(3)
P = product(A, B)                          # A × B = FinSet(6) with proj1 (fst), proj2 (snd)
f = FinFunction([1, 2, 1], 2); g = FinFunction([3, 3, 1], 3)
h = pair(P, f, g)                          # ⟨f, g⟩ : 3 → 6
force(compose(h, proj1(P))) == f           # computation rule
# tuple arithmetic in Julia: swap, assoc
swap((x, y)) = (y, x); assoc(((a, b), c)) = (a, (b, c))
```
tab: Lean
```lean
import Mathlib
#check @Prod.mk                -- introduction: (a, b)
#check @Prod.fst               -- elimination
#check @Prod.map               -- functoriality f × g
#check @Equiv.prodComm         -- α × β ≃ β × α
#check @Equiv.prodAssoc
#check @Equiv.punitProd        -- PUnit × α ≃ α  (left unitor)
#check @CategoryTheory.CartesianMonoidalCategory   -- products as a monoidal structure
```
tab: Haskell
```haskell
mapIn :: (c -> a, c -> b) -> (c -> (a, b))          -- introduction rule
mapIn (f, g) = \c -> (f c, g c)
(&&&) :: (c -> a) -> (c -> b) -> (c -> (a, b))      -- Haskell-style
(f &&& g) c = (f c, g c)
fork :: (c -> (a, b)) -> (c -> a, c -> b)            -- the other direction
fork h = (fst . h, snd . h)
swap :: (a, b) -> (b, a)
swap (x, y) = (y, x)
assoc :: ((a, b), c) -> (a, (b, c))
assoc ((a, b), c) = (a, (b, c))
runit :: (a, ()) -> a
runit (a, _) = a
bimap :: (a -> a') -> (b -> b') -> (a, b) -> (a', b')
bimap f g (a, b) = (f a, g b)
```
````
