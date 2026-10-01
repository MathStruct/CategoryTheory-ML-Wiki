#definition #theorem #example #program

A **linear–non-linear (LNL) adjunction** (Benton) is a symmetric monoidal [[Adjunction]]

$$
\mathcal C \;\underset{G}{\overset{F}{\rightleftarrows}}\; \mathcal L, \qquad F \dashv G,
$$

between a cartesian closed category $\mathcal C$ of **ordinary, duplicable** values and a symmetric monoidal closed category $(\mathcal L, \otimes, \multimap)$ of **linear** ones — resources that must be used exactly once — where $F$ is strong monoidal ($F(A \times B) \cong FA \otimes FB$). It is the categorical model of **intuitionistic linear logic**: the *of course* modality is the induced comonad

$$
!A \;=\; F G A \quad \text{on } \mathcal L,
$$

and a proof that may use a hypothesis any number of times is a linear proof from $!A$. Copying and discarding exist in $\mathcal C$ (it is cartesian: every object has a natural diagonal and a map to $1$) but not in $\mathcal L$; the adjunction says exactly when a linear resource may be treated as an ordinary value — when it is of the form $FA$.

> Sources: Benton, *A mixed linear and non-linear logic: proofs, terms and models*, CSL 1994 (LNCS 933, 1995); Girard, *Linear logic*, Theor. Comput. Sci. 50 (1987); Bierman, *What is a categorical model of intuitionistic linear logic?*, TLCA 1995; Melliès, *Categorical semantics of linear logic*, in *Interactive Models of Computation and Program Behaviour* (Panoramas et Synthèses 27, SMF 2009); Wadler, *Linear types can change the world!*, IFIP TC2 1990. See [[Monoidal Closed Category]] and [[Symmetric Monoidal Category]] for the linear side, [[Comonad]] for $!$, and [[Discard and Copy Axioms]] / [[Copy-Discard Category]] for the structure that the linear side lacks.

## The standard example

$\mathcal C = \mathbf{Set}$, $\mathcal L = \mathbf{Vect}$, $F$ = free vector space, $G$ = underlying set. Copying a *vector*, $v \mapsto v \otimes v$, is not linear. Copying *basis* vectors, $e_i \mapsto e_i \otimes e_i$, is linear, but it is natural only with respect to linear maps of the form $F(f)$ — those that send basis vectors to basis vectors — and fails for a general linear map. So $FA$ carries a comonoid (copy and delete on the basis), the comonoid structure is natural for maps in the image of $F$, and a generic vector space has none: the no-cloning phenomenon, in its simplest classical form.

## Linear, affine, relevant

| allowed structural rule | logic / type system | categorical model |
|---|---|---|
| copy and discard | intuitionistic / ordinary types | cartesian closed category |
| discard only (weakening) | **affine** types (Rust ownership, uniqueness) | symmetric monoidal closed with a natural $A \to I$ (semicartesian) |
| copy only (contraction) | relevant logic | monoidal closed with a natural diagonal |
| neither | **linear** types | symmetric monoidal closed category |

Rust's ownership is affine — values may be dropped but not implicitly copied — with `Copy` types playing the role of $!A$ and borrows (`&T`, `&mut T`) as regionally scoped, non-owning access. Lean and Clean use **uniqueness** (a reference count of one) to perform functional updates in place: affine reasoning used for performance rather than safety.

## Sophia

Sophia must unify four memory models — Julia's tracing GC, C++'s manual RAII, Rust's affine ownership, Lean's reference counting — with regions and borrow forms in the Core Calculus. Categorically, the GC languages live in the cartesian world (values may be shared freely), Rust in the affine one, and moving code from Julia to Rust is moving from $\mathcal C$ into $\mathcal L$: possible only for values in the image of $F$, and otherwise a **refinement** rather than an equivalence — which is the design notes' conclusion, reached here from the structural rules. See [Effects Memory and Resources](https://mathstruct.org/Sophia/vault/Design/Effects-Memory-and-Resources) and [Linear and Affine Types](https://mathstruct.org/Sophia/vault/Background/Type-Theory/Linear-and-Affine-Types).

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# Linear side: finite-dimensional vector spaces, ⊗ = kron. Non-linear side: finite sets.
# F : Set → Vect sends a set to the free vector space on it, a function f to the 0/1 matrix F(f).
F(f, n, m) = [f(j) == i ? 1 : 0 for i in 1:m, j in 1:n]     # matrix of the linear map e_j ↦ e_{f(j)}
# "Copy" on the free space ℝ^n, defined on the basis: δ(e_i) = e_i ⊗ e_i. It is linear …
δ(n) = [i == (j - 1) * n + j ? 1 : 0 for i in 1:n^2, j in 1:n]
v = [1, 2]
δ(2) * v                                                     # [1, 0, 0, 2]: not v ⊗ v = [1, 2, 2, 4]
δ(2) * v == kron(v, v)                                       # false: copying a *vector* is not linear
# … and natural for maps that come from functions (F(f) sends basis vectors to basis vectors):
f = j -> [2, 2, 1][j]; Ff = F(f, 3, 2)
δ(2) * Ff == kron(Ff, Ff) * δ(3)                             # true: duplicable, as a non-linear value
# … but not for a general linear map: resources cannot be copied in the linear world.
A = [1 1; 0 1]
δ(2) * A == kron(A, A) * δ(2)                                # false
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
-- The archetypal LNL adjunction: free module ⊣ forgetful, between Type (non-linear) and R-modules (linear).
#check @ModuleCat.adj      -- ModuleCat.free R ⊣ forget (ModuleCat R)
-- the comonad ! = F ∘ G on modules, induced by the adjunction
noncomputable example (R : Type) [Ring R] : Comonad (ModuleCat R) := (ModuleCat.adj R).toComonad
```
````
