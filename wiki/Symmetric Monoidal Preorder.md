#definition #example

A **symmetric monoidal structure** on a [[Preorder]] $(X, \leq)$ consists of

(i) an element $I \in X$, the **monoidal unit**, and
(ii) a function $\otimes : X \times X \to X$, the **monoidal product**, written $x_1 \otimes x_2$,

satisfying, for all $x, x_i, y, y_i, z \in X$:

(a) **monotonicity**: if $x_1 \leq y_1$ and $x_2 \leq y_2$ then $x_1 \otimes x_2 \leq y_1 \otimes y_2$;
(b) **unitality**: $I \otimes x = x = x \otimes I$;
(c) **associativity**: $(x \otimes y) \otimes z = x \otimes (y \otimes z)$;
(d) **symmetry**: $x \otimes y = y \otimes x$.

A preorder with such a structure, $(X, \leq, I, \otimes)$, is a **symmetric monoidal preorder**. Replacing $=$ by $\cong$ throughout gives a **weak** monoidal structure (Remark 2.3). Notation varies: units $I, 0, 1, \mathsf{true}, \mathsf{false}, \{\ast\}$; products $\otimes, +, \ast, \wedge, \vee, \times$.

> Sources: 7 Sketches Definition 2.2, Remark 2.3, Examples 2.4, 2.6, 2.9, 2.27, 2.30, 2.32, 2.37, Proposition 2.38; DaoFP §5.3 ("Monoidal Category"), §20.1; 7 Sketches §4.4.3 (the categorification: [[Monoidal Category]]).

Anyone can propose $(X, \leq, I, \otimes)$; it is a symmetric monoidal preorder iff (a)–(d) hold. Chapter 2 of 7 Sketches reads $x \leq y$ as "resource $x$ can be converted into resource $y$" and $x \otimes y$ as "having both $x$ and $y$" ([[Resource Theory]]); [[Wiring Diagrams for Monoidal Preorders|wiring diagrams]] are the graphical language.

## Examples

| preorder | unit | product | note |
|---|---|---|---|
| $(\mathbb{R}, \leq, 0, +)$ | $0$ | $+$ | Example 2.4; $(\mathbb{R}, \leq, 1, \ast)$ fails monotonicity ([[7S Exercise 2.5]]) |
| $(\mathrm{Disc}_M, =, e, \ast)$ for a commutative [[Monoid]] $M$ | $e$ | $\ast$ | Example 2.6, [[7S Exercise 2.8]] |
| [[Bool (Monoidal Preorder)|$\mathbf{Bool}$]] $= (\mathbb{B}, \leq, \mathsf{true}, \wedge)$ | $\mathsf{true}$ | $\wedge$ | Example 2.27; also $(\mathbb{B}, \leq, \mathsf{false}, \vee)$ ([[7S Exercise 2.29]]) |
| $(\mathbb{N}, \leq, 0, +)$ and $(\mathbb{N}, \leq, 1, \ast)$ | | | Example 2.30, [[7S Exercise 2.31]], [[7S Exercise 2.45]] |
| $(\mathbb{N}, \mid, 1, \ast)$ | $1$ | $\ast$ | Example 2.32 ([[Divisibility Order]]); $(\mathbb{N}, \mid, 0, +)$ fails ([[7S Exercise 2.33]]) |
| $\mathbf{NMY} = (\{\mathsf{no} \leq \mathsf{maybe} \leq \mathsf{yes}\}, \mathsf{yes}, \min)$ | | | [[7S Exercise 2.34]] |
| $(\mathcal{P}(S), \subseteq, S, \cap)$ | $S$ | $\cap$ | [[7S Exercise 2.35]]; a [[Quantale]] |
| $\mathrm{Prop}_{\mathbb{N}}$, statements about $n$ ordered by implication | $\mathsf{true}$ | $\wedge$ | [[7S Exercise 2.36]] |
| [[Cost]] $= ([0, \infty], \geq, 0, +)$ | $0$ | $+$ | Example 2.37 (Lawvere) |
| $(\mathrm{Mat}, \to, 0, +)$, chemical materials and reactions | $0$ | $+$ | [[Resource Theory]] |
| $\mathbf{W} = (\mathbb{N} \cup \{\infty\}, \leq, \infty, \min)$ | $\infty$ | $\min$ | [[7S Exercise 2.63]] |

**Non-example** (Example 2.9): poker hands ordered by strength, with $h_1 \otimes h_2$ = "best hand from the ten cards", fails monotonicity: $h_1 \leq i_1$, $h_2 \leq i_2$ but $h_1 \otimes h_2$ can be a royal flush beating $i_1 \otimes i_2$.

## Constructions and uses

- The [[Opposite Monoidal Preorder]] $(X, \geq, I, \otimes)$ is again symmetric monoidal (Proposition 2.38).
- Structure-preserving maps are [[Monoidal Monotone Map|monoidal monotones]].
- A symmetric monoidal preorder $\mathcal{V}$ is a *base of enrichment*: $\mathcal{V}$-categories ([[Enriched Category]]) let $\mathcal{V}$ "structure the question of getting from $a$ to $b$". Symmetry is needed for [[Product of Enriched Categories|products]] ([[7S Exercise 2.75]]).
- Extra axioms give different [[Wiring Diagram|wiring-diagram styles]]: the [[Discard and Copy Axioms|discard axiom]] $x \leq I$ (manufacturing) and copy axiom $x \leq x \otimes x$ (informatics).
- A symmetric monoidal preorder is exactly a thin [[Symmetric Monoidal Category]]; a *closed* one is a [[Monoidal Closed Preorder]], and one with all joins is a [[Quantale]]. Ordered commutative monoids in the algebra literature are the skeletal case.

````tabs
tab: Julia
```julia
# Catlab: thin symmetric monoidal categories = symmetric monoidal preorders
using Catlab
x, y, z = Ob(FreeThinSymmetricMonoidalCategory, :x, :y, :z)
f, g = Hom(:f, x, y), Hom(:g, y, z)           # f witnesses x ≤ y
f ⊗ g                                          # x ⊗ y ≤ y ⊗ z   (monotonicity)
dom(f ⊗ g) == x ⊗ y                            # true
munit(FreeThinSymmetricMonoidalCategory.Ob)    # the unit I

# a concrete instance as a Kittenlab-style preorder with unit and product
struct RealPlus <: Preorder{Float64} end
leq(::RealPlus, a, b) = a <= b
otimes(::RealPlus, a, b) = a + b
munit(::RealPlus) = 0.0
```
tab: Lean
```lean
-- Mathlib: an ordered (additive) commutative monoid is a skeletal symmetric monoidal preorder
example : OrderedAddCommMonoid ℝ := inferInstance          -- (ℝ, ≤, 0, +)
example : OrderedCommMonoid ℕ := inferInstance             -- (ℕ, ≤, 1, *)
#check @add_le_add   -- monotonicity: a ≤ b → c ≤ d → a + c ≤ b + d
-- as a thin symmetric monoidal category: a preorder is a category, and Mathlib's
-- `CategoryTheory.MonoidalCategory` can be instantiated on it (see Monoidal Category)
```
tab: Haskell
```haskell
-- a symmetric monoidal preorder: a Preorder with a commutative Monoid such that (<>) is monotone
class (Preorder a, Monoid a) => MonoidalPreorder a
-- laws: leq x1 y1 && leq x2 y2 ==> leq (x1 <> x2) (y1 <> y2); x <> y == y <> x

import Data.Monoid (Sum(..))
instance Preorder (Sum Double) where leq (Sum a) (Sum b) = a <= b
instance MonoidalPreorder (Sum Double)        -- (ℝ, ≤, 0, +)
```
````
