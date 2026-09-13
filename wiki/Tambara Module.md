#definition #theorem #example #program

A **Tambara module** on a [[Monoidal Category]] $(\mathcal{C}, \otimes, I)$ (originally: cartesian, $\otimes = \times$) is a [[Profunctor]] $P$ equipped with a family of maps

$$
\alpha_{\langle a, b\rangle, c} : P\langle a, b\rangle \to P\langle c \otimes a, c \otimes b\rangle,
$$

natural in $a, b$ and *dinatural* in $c$ (since $c$ appears both co- and contravariantly), preserving the monoidal structure: $\alpha_{\langle a,b\rangle, I} = \mathrm{id}$ and $\alpha_{\langle a,b\rangle, c' \otimes c} \cong \alpha_{\langle c \otimes a, c \otimes b\rangle, c'} \circ \alpha_{\langle a,b\rangle, c}$. Morphisms of Tambara modules are natural transformations $\rho : P \to Q$ commuting with the $\alpha$'s; they form the **Tambara category** $\mathcal{T}$. In Haskell (cartesian case, class `Strong`/`Cartesian`):
```haskell
class Profunctor p => Cartesian p where
  alpha :: p a b -> p (c, a) (c, b)      -- parametricity supplies all the (di)naturality
```

> Sources: DaoFP Chapter 18 ("Tambara Modules": §18.2 "Profunctor Lenses": "Profunctors and lenses", "Tambara module", "Profunctor lenses", "Profunctor lenses in Haskell"; §18.3 "General Optics"; §18.4 "Mixed Optics"), Exercises 18.2.1, 18.4.1; §17.8 (Arrows = pre-arrows that are Tambara modules).

**Why they matter.** To turn the [[Existential Lens]] $\int^c \mathcal{C}(s, c \times a) \times \mathcal{C}(c \times b, t)$ into a representation "map $P\langle a, b\rangle$ to $P\langle s, t\rangle$", one can lift $\langle f, g\rangle$ by $P\langle f, g\rangle : P\langle c \times a, c \times b\rangle \to P\langle s, t\rangle$ — but first needs $P\langle a, b\rangle \to P\langle c \times a, c \times b\rangle$. That missing map *is* the Tambara structure.

**Comonad/monad picture.** $(\Theta P)\langle a, b\rangle := \int_c P\langle c \times a, c \times b\rangle$ is a [[Comonad]] on profunctors ($\varepsilon$ projects at $c = 1$), and Tambara modules are exactly its comonad coalgebras (a coalgebra $P \to \Theta P$ is, by continuity of hom, a dinatural family $\alpha$). Its left adjoint is the [[Monad]]

$$
(\Phi P)\langle s, t\rangle = \int^{u, v, c} \mathcal{C}(s, c \times u) \times \mathcal{C}(c \times v, t) \times P\langle u, v\rangle,
$$

with $[\mathcal{C}^{\mathrm{op}} \times \mathcal{C}, \mathbf{Set}](\Phi P, Q) \cong [\dots](P, \Theta Q)$; so the Tambara category is the [[Eilenberg-Moore Category]] of $\Phi$, giving the free/forgetful adjunction needed for [[Tannakian Reconstruction]]. Evaluating $\Phi$ on the representable $(\mathcal{C}^{\mathrm{op}} \times \mathcal{C})(\langle a, b\rangle, -)$ and applying co-Yoneda returns exactly the existential lens — hence [[Profunctor Optics]].

- **Generalizations**: for $\otimes = +$ the Tambara modules are `Cocartesian`/`Choice` (`alpha' :: p a b -> p (Either c a) (Either c b)`) and the optics are prisms; for the action $c \bullet a = \sum_m c_m \times a^m$ of the [[Day Convolution|Day-monoidal]] category $[\mathbb{N}, \mathcal{C}]$ they give traversals; any action of a monoidal category $\mathcal{M}$ on $\mathcal{C}$ (an *actegory*), or two actions on $\mathcal{C}$ and $\mathcal{D}$, gives *mixed optics* $\int^{m} \mathcal{C}(s, m \bullet a) \times \mathcal{D}(m \bullet b, t)$ ([[DaoFP Chapter 18 Exercises#Exercise 18.4.1|DaoFP Exercise 18.4.1]]).
- Haskell's `Arrow` is a [[Prearrow]] that is also a Tambara module (`first`).

````tabs
tab: Julia
```julia
# a Tambara structure on the function profunctor (->): alpha f = (c, a) -> (c, f(a))
alpha(f) = ((c, a),) -> (c, f(a))
alpha(x -> x + 1)((:tag, 41))          # (:tag, 42)
# on the "get/set" profunctor FlipLens a b s t = (s -> a, s -> b -> t):
alpha_lens((get, set)) = (((c, s),) -> get(s), ((c, s), b) -> (c, set(s, b)))
```
tab: Lean
```lean
import Mathlib
-- Tambara modules as a structure on Type-valued profunctors (cartesian case)
structure Tambara (p : Type → Type → Type) where
  dimap : (s → a) → (b → t) → p a b → p s t
  alpha : p a b → p (c × a) (c × b)
def tambaraFun : Tambara (fun a b => a → b) :=
  { dimap := fun f g h => g ∘ h ∘ f, alpha := fun h ⟨c, a⟩ => (c, h a) }
```
tab: Haskell
```haskell
class Profunctor p where
  dimap :: (s -> a) -> (b -> t) -> p a b -> p s t
class Profunctor p => Cartesian p where           -- Tambara modules for (×); library name: Strong
  alpha :: p a b -> p (c, a) (c, b)
class Profunctor p => Cocartesian p where         -- Tambara modules for (+); library name: Choice
  alpha' :: p a b -> p (Either c a) (Either c b)

instance Profunctor (->) where dimap f g h = g . h . f
instance Cartesian (->) where alpha h (c, a) = (c, h a)
instance Cocartesian (->) where alpha' h = fmap h    -- Either c is a functor
```
````
