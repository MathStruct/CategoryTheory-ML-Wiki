#definition #example #theorem

An **action** of a [[Group]] $G$ on a set $X$ is a [[Monoid Action]] of the underlying monoid: $\circlearrowleft\ : G \times X \to X$ with $e \circlearrowleft x = x$ and $g \circlearrowleft (h \circlearrowleft x) = (gh) \circlearrowleft x$. Because every $g$ has an inverse, each map $g \circlearrowleft - : X \to X$ is a bijection — "when a group acts on a set, it has the character of **symmetry**". The **orbit** of $x$ is $Gx := \{g \circlearrowleft x \mid g \in G\}$; "being in the same orbit" is an [[Equivalence Relation]] (reflexive by $e$, symmetric by inverses, transitive by composition), so the orbits [[Partition|partition]] $X$ ([[CTfS Chapter 3 Exercises#Exercise 3.2.1.15|CTfS Exercise 3.2.1.15]]). Categorically, an action is a [[Functor]] $G \to \mathbf{Set}$ from the one-object [[Groupoid]] $G$, and the set of orbits is its [[Colimit]].

> Sources: CTfS §3.2 (Definitions 3.2.1.9, 3.2.1.12, Examples 3.2.1.4, 3.2.1.10, Applications 3.2.1.6, 3.2.1.13, Exercises 3.2.1.11, 3.2.1.14–3.2.1.15), Slogan 4.2.1.5; §5.2.2.1 (representations: actions on vector spaces).

## Examples from Category Theory for Scientists

- **Rotations of the earth** (CTfS Example 3.2.1.10). The circle group $U(1)$ — angles with $360° = 0°$, or unit complex numbers under multiplication — acts on $\mathbb R^3$ by rotation about the $z$-axis,
  $$\theta \circlearrowleft (x, y, z) = (x \cos\theta + y \sin\theta,\ -x \sin\theta + y\cos\theta,\ z),$$
  and preserves the unit sphere since $(x\cos\theta + y\sin\theta)^2 + (-x\sin\theta + y\cos\theta)^2 + z^2 = x^2 + y^2 + z^2$. An action table needs infinitely many columns (one per angle); with this formula $(1,0,0) \mapsto (0.71, -0.71, 0)$ at $45°$ and $(3,4,2) \mapsto (4, -3, 2)$ at $90°$ (CTfS's printed table rotates the other way).
- **Latitudes and climate** (CTfS Application 3.2.1.13). The orbits of $U(1)$ on the earth's surface are the circles of latitude; on a thin atmospheric shell they are latitude-lines-at-altitude. "A simplifying assumption in climatology may be given by assuming that $U(1)$ acts on all currents in the atmosphere": only motion that looks the same along each orbit is allowed. The orbits on all of $\mathbb R^3$ are the horizontal circles around the $z$-axis together with the single points on it ([[CTfS Chapter 3 Exercises#Exercise 3.2.1.14|CTfS Exercise 3.2.1.14]]).
- **Permutations** (CTfS Exercises 3.2.1.7, 3.2.1.11): the permutations $\Sigma_X$ of a set form a group acting on $X$ by evaluation, $\sigma \circlearrowleft x = \sigma(x)$; $\Sigma_{\{1,2,3\}}$ has a single orbit. Every action of $G$ on $X$ *is* a homomorphism $G \to \Sigma_X$ (Cayley's view).
- **Symmetries of a square and of crystals** (CTfS Example 3.2.1.4, Application 3.2.1.6): the dihedral group of 8 symmetries acts on the square; the *space group* of an arrangement of atoms $A \subseteq \mathbb R^3$ is the group of isometries $f$ of $\mathbb R^3$ with $f(A) \subseteq A$ — the dashed arrow in $A \to A$ over $f : \mathbb R^3 \to \mathbb R^3$.
- **Linear symmetries**: $GL_3$ acts on $\mathbb R^3$, $O_3$ on the sphere, the Euclidean group $E(3)$ on space. Actions on vector spaces by linear maps are *representations*, functors $G \to \mathbf{Vect}$ (CTfS §5.2.2.1).

## Structure

- **Monoids vs. groups** (CTfS §3.2): "monoids are likely useful in thinking about diffusion, in which time plays a role and things cannot be undone; groups in mechanics, where actions are time-reversible." When a symmetry breaks, forget along $\mathbf{Grp} \to \mathbf{Mon}$ and keep the monoid action (CTfS Application 4.1.2.4).
- The orbit set is the [[Coequalizer]] of the two maps $G \times X \rightrightarrows X$ (action and projection), i.e. the colimit of the functor $G \to \mathbf{Set}$; the set of fixed points is its [[Limit]].
- Equivariant maps are [[Natural Transformation|natural transformations]]; $G$-sets form a [[Topos]] $\mathbf{Set}^G$, and the [[Endomorphism Monoid|automorphism group]] $\mathrm{Aut}(X)$ of any object of any category acts on its hom-sets.

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
using LinearAlgebra
# U(1) acting on ℝ³ by rotation about the z-axis (CTfS Example 3.2.1.10)
R(θ) = [cosd(θ) sind(θ) 0; -sind(θ) cosd(θ) 0; 0 0 1]
act(θ, p) = R(θ) * p
round.(act(45, [1, 0, 0]); digits = 2)                 # [0.71, -0.71, 0.0] (sign convention of R)
act(190, act(278, [3, 4, 2])) ≈ act(468, [3, 4, 2])     # the action law: θ₁ ⋅ (θ₂ ⋅ p) = (θ₁+θ₂) ⋅ p
norm(act(100, [0.6, 0.0, 0.8])) ≈ 1                      # the sphere is preserved
# the orbit of a point on the sphere is its circle of latitude
orbit(p; n = 8) = [act(360k / n, p) for k in 0:n-1]
all(q -> q[3] ≈ 0.8, orbit([0.6, 0.0, 0.8]))            # same height z: true
```
tab: Lean
```lean
import Mathlib
#check @MulAction.orbit           -- the orbit G • x
#check @MulAction.orbitRel        -- "same orbit" as a Setoid (equivalence relation)
#check @MulAction.orbitRel.Quotient
#check @MulAction.stabilizer
#check @MulAction.toPermHom       -- an action is a homomorphism G →* Equiv.Perm X
example (X : Type) : MulAction (Equiv.Perm X) X := inferInstance   -- Σ_X acts on X
```
tab: Haskell
```haskell
import Data.List (nub, sort)

-- the dihedral group of the square acting on its corners 0..3
data D4 = Rot Int | Flip Int deriving (Eq, Show)   -- ρ^k and φ ρ^k
actD4 :: D4 -> Int -> Int
actD4 (Rot k)  c = (c + k) `mod` 4
actD4 (Flip k) c = (negate (c + k)) `mod` 4

orbit :: [g] -> (g -> x -> x) -> x -> [x]
orbit gs act x = map (`act` x) gs

-- orbit [Rot k | k <- [0..3]] actD4 0 == [0,1,2,3]: one orbit, the action is transitive
```
````
