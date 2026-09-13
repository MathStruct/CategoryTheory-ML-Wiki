#definition #theorem #example #program

"The first rule of category-theory club is that you don't talk about the internals of objects; the second rule is that, if you have to, use arrows only." A composite object $s$ can be described as splitting into a **focus** $a$ and a **residue** $c$ whose type we do not care about — only that it recombines with the focus. This is the **existential lens**
```haskell
data LensE s t a b where
  LensE :: (s -> (c, a)) -> ((c, b) -> t) -> LensE s t a b     -- forward and backward pass
```
categorically the [[Coend]]

$$
\mathcal{L}\langle s, t\rangle\langle a, b\rangle = \int^{c} \mathcal{C}(s, c \times a) \times \mathcal{C}(c \times b, t),
$$

a *type-changing* [[Lens]]: replacing the focus $a$ by $b$ turns the whole $s$ into $t$. The integrand is a profunctor in $\langle x, y\rangle$ via $\mathcal{C}(s, y \times a) \times \mathcal{C}(x \times b, t)$ ([[DaoFP Chapter 17 Exercises#Exercise 17.9.1|DaoFP Exercise 17.9.1]]).

> Sources: DaoFP §17.9 ("Existential Lens": "Existential lens in Haskell", "Existential lens in category theory", "Type-changing lens in Haskell", "Lens composition", "Category of lenses"), §17.10 ("Lenses and Fibrations"), Exercise 17.9.1; §18 ([[Tambara Module|Tambara modules]] give the practical representation).

- `get = snd . l`, `set s b = r (fst (l s), b)`: mediating between the producer `l` and consumer `r` of the residue never exposes `c` (`getResidue` does not type-check). The backward pass answers "what change to the input produces a given change of the focus" — the viewpoint used for lenses in neural networks / [[Open Graph|open learners]].
- `prodLens = LensE id id :: LensE (c, a) (c, b) a b` focuses on the second component of a pair.
- **Composition** takes the product of residues: `compLens (LensE l2 r2) (LensE l1 r1) = LensE (assoc' . bimap id l2 . l1) (r1 . bimap id r2 . assoc)`; e.g. `compLens prodLens prodLens` on `("Outer", (True, 42))` gets `42` and can set it to `'z'`. Lenses form the category $\mathbf{Lens}$ with objects pairs $\langle s, t\rangle$ — but the coend formula for composition is too clumsy in practice, which motivates profunctor optics.
- **Fibrational view** (§17.10): `get` is a projection $p : E \to B$ of a bundle and `set` a *transport* $q : E \times B \to E$ to a new fiber; the lens laws become the *transport law* $p \circ q = \pi_2$, the *identity law* $q \circ (\mathrm{id} \times p) \circ \delta = \mathrm{id}$ and the *composition law* $q \circ (q \times \mathrm{id}) = q \circ (\mathrm{id} \times \varepsilon \times \mathrm{id})$, written with the comonoid $(\delta, \varepsilon)$ of $E$ so as to generalize to monoidal categories. Type-changing lenses transport between a family of bundles fibered over a category of focus types.

````tabs
tab: Julia
```julia
# an existential lens as a forward/backward pair; composition takes the product of residues
struct LensE; l::Function; r::Function; end          # l : s -> (c, a),  r : (c, b) -> t
get(ln::LensE) = s -> ln.l(s)[2]
set(ln::LensE) = (s, b) -> ln.r((ln.l(s)[1], b))
prodLens = LensE(identity, identity)                 # (c, a) ↦ (c, b)
comp(l2::LensE, l1::LensE) = LensE(
    s -> ((c, a) = l1.l(s); (c′, a′) = l2.l(a); ((c, c′), a′)),
    ((cc, b′),) -> l1.r((cc[1], l2.r((cc[2], b′)))))
l3 = comp(prodLens, prodLens)
x = ("Outer", (true, 42))
get(l3)(x), set(l3)(x, 'z')                          # (42, ("Outer", (true, 'z')))
```
tab: Lean
```lean
import Mathlib
structure LensE (s t a b : Type) where
  {c : Type}
  fwd : s → c × a
  bwd : c × b → t
def LensE.get (l : LensE s t a b) : s → a := fun x => (l.fwd x).2
def LensE.set (l : LensE s t a b) : s → b → t := fun x y => l.bwd ((l.fwd x).1, y)
```
tab: Haskell
```haskell
{-# LANGUAGE GADTs #-}
import Data.Bifunctor (bimap)
data LensE s t a b where
  LensE :: (s -> (c, a)) -> ((c, b) -> t) -> LensE s t a b
toGet :: LensE s t a b -> (s -> a)
toGet (LensE l _) = snd . l
toSet :: LensE s t a b -> (s -> b -> t)
toSet (LensE l r) s b = r (fst (l s), b)
prodLens :: LensE (c, a) (c, b) a b
prodLens = LensE id id
compLens :: LensE a b a' b' -> LensE s t a b -> LensE s t a' b'
compLens (LensE l2 r2) (LensE l1 r1) = LensE l3 r3
  where l3 = assoc' . bimap id l2 . l1
        r3 = r1 . bimap id r2 . assoc
        assoc ((c, c'), b') = (c, (c', b'))
        assoc' (c, (c', a')) = ((c, c'), a')
```
````
