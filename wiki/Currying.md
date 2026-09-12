#definition #theorem #example

**Currying** (after Haskell Curry) is the bijection between functions of two variables and functions returning functions:
$$\mathbf{Set}(A \times B, C) \cong \mathbf{Set}(A, C^B), \qquad f \mapsto (a \mapsto (b \mapsto f(a, b))).$$
"If I have a function $f$ of two variables $a, b$, I can put off entering the second variable: if you give me just $a$, I'll return a function $B \to C$ that's waiting for the $B$ input." Categorically it is the [[Adjunction]] $(- \times B) \dashv (-)^B$ defining the [[Exponential Object]]; in general categories it is the defining property of a [[Cartesian Closed Category]], and $\mathbf{Cat}$ is one: $\mathbf{Cat}(\mathcal{C} \times \mathcal{D}, \mathcal{E}) \cong \mathbf{Cat}(\mathcal{C}, [\mathcal{D}, \mathcal{E}])$ (functors can be curried, DaoFP §9.7).

> Sources: 7 Sketches Example 3.72, Exercise 3.73; DaoFP Chapter 6 ("Currying", "Relation to lambda calculus"), §10.1 ("The Currying Adjunction"), §10.5; Kittenlab (implicit: Julia closures).

- [[7S Exercise 3.73]]: $(- \times B)$ acts on morphisms by $f \times B : (x, b) \mapsto (f(x), b)$; $(-)^B$ acts by $f^B : g \mapsto g \mathbin{;} f$; currying $+ : \mathbb{N} \times \mathbb{N} \to \mathbb{N}$ gives $p(3) = (n \mapsto n + 3)$.
- **Lambda calculus** (DaoFP §6): a term $\Gamma, x : a \vdash e : b$ corresponds to an arrow $\Gamma \times a \to b$; $\lambda$-abstraction is currying, application is the counit $\varepsilon$; $\beta$-reduction and $\eta$-conversion are the two triangle identities. Haskell functions are curried by default: `f :: a -> b -> c` is `a -> (b -> c)`.
- [[Unit and Counit of an Adjunction|Unit and counit]]: `unit = curry id :: e -> (a -> (e, a))` and `counit = uncurry id :: (a -> b, a) -> b` (function application).
- The preorder shadow: the hom-element $v \multimap w$ of a [[Monoidal Closed Preorder]], $(a \otimes v) \leq w$ iff $a \leq (v \multimap w)$ — "$a$ and $v$ suffice to get $w$ iff $a$ suffices to get a single-use $v$-to-$w$ converter".

````tabs
tab: Julia
```julia
curry(f) = x -> y -> f(x, y)
uncurry(g) = (x, y) -> g(x)(y)
add3 = curry(+)(3); add3(4)     # 7
```
tab: Lean
```lean
#check @Function.curry     -- (α × β → γ) → α → β → γ
#check @Function.uncurry
#check Equiv.curry         -- (α × β → γ) ≃ (α → β → γ)
#check CategoryTheory.CartesianClosed.curry   -- in any CCC
```
tab: Haskell
```haskell
curry :: ((a, b) -> c) -> (a -> b -> c)
curry f a b = f (a, b)
uncurry :: (a -> b -> c) -> ((a, b) -> c)
uncurry g (a, b) = g a b
-- Exercise 3.73: currying (+) and applying to 3
add3 :: Int -> Int
add3 = curry (uncurry (+)) 3
```
````
