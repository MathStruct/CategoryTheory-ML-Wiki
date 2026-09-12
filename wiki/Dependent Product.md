#definition #theorem #example #program

The **dependent product** (pi type, dependent function type) $\Pi_{x : B}\, T(x)$ is the type of functions whose *return type depends on the argument*: $s : \Pi_{x : B} T(x)$ applied to $x : B$ gives $s(x) : T(x)$. In sets, an element of $\Pi_{x : B} T(x)$ selects one element from each $T(x)$ — a giant tuple indexed by $B$ (for $B = \{1, 2\}$ it is $T(1) \times T(2)$, whence "product"). In the fibration picture, a dependent function is a **section** of the bundle $p : E \to B$: "like a haircut, it cuts through each fiber"; in physics, a *field* over spacetime ([[Sheaf of Sections]]).

> Sources: DaoFP §11.4 ("Dependent Product": "Dependent product in Haskell", "Dependent product of sets", "Dependent product categorically", "Adding the atlas", "Universal quantification"), Exercises 11.4.1–11.4.2; 7 Sketches §7.3.3, §7.4.4.

## The object of sections

Mimicking the [[Exponential Object]] (application $\varepsilon : C^B \times B \to C$), the object of sections $S(E)$ of $\langle E, p \rangle$ has a dependent application $\varepsilon : S(E) \times B \to E$ with $p \circ \varepsilon = \pi_2$ — the value lands in the right fiber — i.e. $\varepsilon$ is a morphism $\langle S(E) \times B, \pi_2 \rangle \to \langle E, p \rangle$ in $\mathcal{C}/B$, universal:
$$(\mathcal{C}/B)\big(\langle G \times B, \pi_2 \rangle, \langle E, p \rangle\big) \cong \mathcal{C}(G, S(E)).$$
Each $y \in G$ cuts a horizontal slice $\{(y, b)\}$ of $G \times B$, which a fiberwise map sends to a section of $E$; so elements of $S(E)$ are exactly sections. The counit is dependent function application.

## Adding the atlas

Replacing $1$ by a base $A$ and $G \times B = {!}^* G$ by the pullback $f^* G$ along $f : B \to A$ gives the definition of $\Pi_f$ as the **right adjoint of the [[Base Change Functor]]**:
$$(\mathcal{C}/B)\big(f^* \langle G, q \rangle, \langle E, p \rangle\big) \cong (\mathcal{C}/A)\big(\langle G, q \rangle, \Pi_f \langle E, p \rangle\big),$$
written $f_* : \mathcal{C}/B \to \mathcal{C}/A$. The fiber of $\Pi_f E$ over $x \in A$ is the set of *partial sections* of $E$ over the patch $f^{-1}(x) \subseteq B$ ([[DaoFP Exercise 11.4.2]]); $f$ localizes sections to neighbourhoods. Altogether $\Sigma_f \dashv f^* \dashv \Pi_f$ in a [[Locally Cartesian Closed Category]].

- **Logic**: $\Pi_{x : B} T(x)$ is $\forall_{x : B}\, T(x)$ — a section proves every $T(x)$ is inhabited ([[Quantification]]). The induction principle for $\mathbb{N}$ produces an element of $\Pi_{n : \mathbb{N}} T(n)$ from $\mathit{init} : T(Z)$ and $\mathit{step} : \Pi_n (T(n) \to T(Sn))$ ([[Natural Numbers Object]]).
- **Haskell** has no $\Pi$; one passes the index as a *singleton* value: `replicateV :: a -> SNat n -> Vec n a` returns a different type for each `n` — an infinite tuple `((), x, (x,x), (x,x,x), ...)`.

````tabs
tab: Julia
```julia
using Catlab
# sections of a finite bundle p : E → B = one element from each fiber (Π_{x:B} p⁻¹(x))
p = FinFunction([1, 1, 2, 3, 3], 3)
fibers = [preimage(p, x) for x in 1:3]
sections = collect(Iterators.product(fibers...))     # 2 · 1 · 2 = 4 sections
length(sections)
# Π_f localizes: for f : B → A, the fiber of Π_f E over y is the sections over f⁻¹(y)
f = FinFunction([1, 1, 2], 2)
[prod(length(fibers[x]) for x in preimage(f, y)) for y in 1:2]   # [2, 2]
```
tab: Lean
```lean
import Mathlib
-- Π types are primitive in Lean: (x : B) → T x
example : (n : ℕ) → Fin (n + 1) := fun n => ⟨0, Nat.succ_pos n⟩   -- a section
#check @CategoryTheory.Over.pullback
-- in `Type`, Π_f is the dependent function type over each fiber; Mathlib's slices Over B are cartesian closed:
example (B : Type) : CategoryTheory.CartesianClosed (CategoryTheory.Over B) := inferInstance
```
tab: Haskell
```haskell
{-# LANGUAGE DataKinds, GADTs #-}
data SNat n where            -- singletons stand in for the value n at the type level
  SZ :: SNat 'Z
  SS :: SNat n -> SNat ('S n)
replicateV :: a -> SNat n -> Vec n a       -- a dependent function: result type depends on n
replicateV _ SZ     = VNil
replicateV x (SS n) = VCons x (replicateV x n)
```
````
