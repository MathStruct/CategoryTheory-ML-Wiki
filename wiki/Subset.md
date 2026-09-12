#definition #example

Given [[Set|sets]] $X$ and $Y$, $X$ is a **subset** of $Y$, written $X \subseteq Y$, if every element of $X$ is in $Y$. The empty set is a subset of every set. Given a property $P$ on $Y$, $\{y \in Y \mid P(y)\}$ is the subset of elements satisfying $P$.

> Sources: 7 Sketches §1.2.1; Kittenlab Lecture 5 & 14.

## Two categorical views (Kittenlab)

1. A subset is a **characteristic function** $\chi : X \to \mathbb{B}$ (the [[Booleans]]); $x \in U$ iff $\chi_U(x) = \mathsf{true}$. This is *canonical*: two characteristic functions describe the same subset iff they are equal.
2. A subset is another set $U$ with an injection $\iota_U : U \hookrightarrow X$ (a [[Monomorphism]]). This generalizes to [[Subobject|subobjects]] in any category, but two injections may describe the same subset while being merely isomorphic (e.g. $\{a,b\} \hookrightarrow \{1,2,3\}$ and $\{1,2\} \hookrightarrow \{1,2,3\}$).

Categories in which an analogue of $\mathbb{B}$ exists — a [[Subobject Classifier]] — are [[Topos|toposes]].

The **Iverson bracket** $[P]$ denotes $\mathsf{true}$ if the statement $P$ holds and $\mathsf{false}$ otherwise, so e.g. the filled parabola $\{(x,y) \mid y \geq x^2\} \subseteq \mathbb{R}^2$ has characteristic function $\chi(x,y) = [y \geq x^2]$. A mathematical model, in the behavioural view of Willems, is exactly a subset of a "universum" of possibilities: an *exclusion law*.

The subsets of $X$ form the [[Power Set]] $\mathcal{P}(X)$, a [[Partial Order|poset]] under inclusion, in which [[Meet|meets]] are intersections and [[Join|joins]] are unions. Functions act on subsets by [[Direct Image, Preimage, and Dual Image|preimage and direct image]].

````tabs
tab: Julia
```julia
# Kittenlab Lecture 14: subsets of {1,…,n} as bit vectors (characteristic functions)
const FinSubset = BitVector
A = FinSubset([true, false, true])   # {1,3} ⊆ {1,2,3}
B = FinSubset([false, true, true])   # {2,3} ⊆ {1,2,3}
A .&& B                               # intersection {3}

# Kittenlab Lecture 5: subset-of test for finite sets
subsetof(U, A) = all(x ∈ A for x in U)

# Catlab: subobjects of a FinSet
using Catlab
X = FinSet(3)
U = Subobject(X, [1, 3])   # {1,3} ↪ {1,2,3}
hom(U)                     # the inclusion FinFunction([1, 2], 2, 3)
```
tab: Lean
```lean
-- `Set α := α → Prop` is literally the characteristic-function view
example (U V : Set ℕ) (h : U ⊆ V) (x : ℕ) (hx : x ∈ U) : x ∈ V := h hx
-- the injection view: the subtype
example (U : Set ℕ) : Type := {x : ℕ // x ∈ U}
#check (Subtype.val : {x : ℕ // x ∈ U} → ℕ)   -- the inclusion ι_U
```
tab: Haskell
```haskell
-- characteristic-function view
type Subset a = a -> Bool

parabola :: Subset (Double, Double)
parabola (x, y) = y >= x * x

intersect :: Subset a -> Subset a -> Subset a
intersect u v x = u x && v x

-- injection view: a "typed" carrier with an injective map into a
data Sub a = forall u. Sub (u -> a)
```
````
