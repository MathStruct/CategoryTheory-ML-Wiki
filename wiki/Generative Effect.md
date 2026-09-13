#definition #example

A [[Monotone Map]] $f : P \to Q$ **has a generative effect** if there exist elements $a, b \in P$ such that

$$
f(a) \vee f(b) \neq f(a \vee b).
$$

Thinking of $f$ as an *observation* of systems $a$ and $b$: the left side combines the observations of the pieces, the right side observes the combined system. A generative effect means "we see something when we observe the combined system that we could not expect by merely combining our observations of the pieces". By [[7S Chapter 1 Exercises#Exercise 1.94|7S Exercise 1.94]] one always has $f(a) \vee f(b) \leq f(a \vee b)$, so the effect is always *more* stuff.

> Sources: 7 Sketches §1.1, §1.3.2, Definition 1.93, Exercises 1.4, 1.6, 1.77; following Adam's thesis [Ada17].

## The motivating example (§1.1.1)

A *system* is a way of connecting three points $\bullet, \circ, \ast$ — a [[Partition]] of $\{\bullet, \circ, \ast\}$ (connection is symmetric and transitive, like contagion, unlike friendship). There are five systems, ordered by "$A \leq B$ if whenever $x$ is connected to $y$ in $A$ then also in $B$" ([[Hasse Diagram]] Eq. (1.5), [[Preorder of Partitions]]). Alice's observation $\Phi$ returns $\mathsf{true}$ iff $\bullet$ is connected to $\ast$; it is monotone $\Phi : \mathrm{Prt}(\{\bullet,\circ,\ast\}) \to \mathbb{B}$ ([[7S Chapter 1 Exercises#Exercise 1.77|7S Exercise 1.77]]). **Joining** systems $A \vee B$ takes the transitive closure of the union of connections — the [[Join]] in $\mathrm{Prt}$.

Take $A = (\bullet\circ)(\ast)$ and $B = (\bullet)(\circ\ast)$. Then $\Phi(A) = \Phi(B) = \mathsf{false}$, so $\Phi(A) \vee \Phi(B) = \mathsf{false}$, but $A \vee B = (\bullet\circ\ast)$ and $\Phi(A \vee B) = \mathsf{true}$: the observation is "inherently lossy" with respect to join, and cannot be fixed without also observing $\circ$. This matters e.g. when two local authorities separately extract information about contagion between an infected $\bullet$ and a vulnerable $\ast$ and combine the results — they get a different answer than combining the raw data first.

## Observations that preserve structure

"Asking which aspects of $X$ one wants to preserve under the observation becomes the question *what category are you working in?*" ([[7S Chapter 1 Exercises#Exercise 1.1|7S Exercise 1.1]]: order-, metric-, addition-preserving maps $\mathbb{R} \to \mathbb{R}$.) $\Phi$ preserves order but not join. The map $\Phi$ hints at [[Category|categories]], [[Functor|functors]], [[Colimit|colimits]] and [[Adjunction|adjunctions]]: left adjoints of [[Galois Connection|Galois connections]] preserve joins, so they *never* have generative effects; a map preserving all joins out of a preorder with all joins is a left adjoint ([[Adjoint Functor Theorem for Preorders]]). Adam's general setting replaces joins by [[Colimit|colimits]] and detects generative effects with abelian categories and cohomology.

````tabs
tab: Julia
```julia
# the five systems as partitions of {•,∘,∗} = {1,2,3}, given by surjections; Φ = "1 ~ 3"
using Catlab
A = FinFunction([1,1,2], 2)      # (•∘)(∗)
B = FinFunction([1,2,2], 2)      # (•)(∘∗)
Φ(c::FinFunction) = c(1) == c(3)
# join of partitions = coequalizer-style transitive closure; here A ∨ B = (•∘∗)
AvB = FinFunction([1,1,1], 1)
(Φ(A) || Φ(B), Φ(AvB))           # (false, true): a generative effect
```
tab: Haskell
```haskell
-- observation Φ on partitions (as equivalence predicates) of {1,2,3}
type Sys = Int -> Int -> Bool
phi :: Sys -> Bool
phi c = c 1 3

sysA, sysB, sysAB :: Sys
sysA x y = x == y || (x, y) `elem` [(1,2),(2,1)]
sysB x y = x == y || (x, y) `elem` [(2,3),(3,2)]
sysAB _ _ = True                       -- the join

-- (phi sysA || phi sysB, phi sysAB) == (False, True)
```
````
