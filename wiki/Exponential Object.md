#definition #example #theorem

In a [[Category]] with [[Product|products]], the **exponential** (**function object**, **internal hom**) $b^a$ (also $[a, b]$ or $a \Rightarrow b$) of objects $a, b$ is an object with an **evaluation** morphism $\varepsilon_{ab} : b^a \times a \to b$ such that for every $f : x \times a \to b$ there is a unique $h : x \to b^a$ (the **curried** $f$) with $f = \varepsilon_{ab} \circ (h \times \mathrm{id}_a)$. Equivalently, a natural isomorphism
$$\mathcal{C}(x \times a, b) \cong \mathcal{C}(x, b^a),$$
i.e. $(- \times a) \dashv (-)^a$ — the [[Currying|currying adjunction]]. A category with all exponentials (and finite products) is a [[Cartesian Closed Category]].

> Sources: DaoFP §1.4 ("The Object of Arrows"), Chapter 6 ("Function Types": elimination rule, introduction rule, currying, modus ponens, functoriality), §9.4 ("Exponentials"), §10.1, §10.5; 7 Sketches Example 3.72 ($C^B$ in $\mathbf{Set}$, $|C^B| = |C|^{|B|}$), Exercise 3.73, §7.2.1; Definition 2.79 ([[Monoidal Closed Preorder|hom-elements]] are the preorder shadow).

## Examples and rules (DaoFP Chapter 6)

- In $\mathbf{Set}$, $C^B$ is the set of functions $B \to C$; in Haskell `a -> b`. "We have arrows which connect $a$ and $b$ — these form a set; and we have an *object* of arrows, whose elements are arrows from the terminal object": `f :: a -> b` is $1 \to b^a$. In logic $B^A$ is the *proposition* "if $A$ then $B$" — an element of it is a proof (the counterfactual "if wishes were horses, beggars would ride" has a proof but an unprovable premise).
- **Elimination rule**: evaluation/`apply :: (a -> b, a) -> b`, *modus ponens*. **Introduction rule**: currying, `curry :: ((x, a) -> b) -> (x -> a -> b)`, from a two-argument function to a function returning a function. A function of $\Gamma \times a \to b$ is an expression in environment $\Gamma$ with a free variable of type $a$; the exponential is a *closure* capturing $\Gamma$ (DaoFP §10.1).
- **Yoneda trick** (DaoFP §9.4): substituting $x := b^a$ and picking $h = \mathrm{id}$, the commuting condition gives $\varepsilon_{ab} = \alpha^{-1}(\mathrm{id}) =$ `uncurry id`, and the naturality square then yields $\varepsilon \circ (h \times \mathrm{id}) = f$. The unit of the adjunction is $\eta : e \to (e \times a)^a$, `curry id`.
- **Functoriality**: $b^a$ is covariant in $b$ and contravariant in $a$ — a [[Profunctor]], `dimap f g h = g . h . f`; "the function object can be visualized as a lookup table keyed by $a$: to use a related key $a'$ you need a converter $a' \to a$". Sums and products revisited: $x^{a + b} \cong x^a \times x^b$, $(a \times b)^x \cong a^x \times b^x$, $(b^a)^x \cong b^{a \times x}$, $b^1 \cong b$, $b^0 \cong 1$.
- In a [[Bicartesian Closed Category]] products distribute over sums, since $(- \times a)$ is a left adjoint ([[Right Adjoints Preserve Limits|left adjoints preserve colimits]]).
- $b^a$ is the [[Representable Functor|representing object]] of $x \mapsto \mathcal{C}(x \times a, b)$; the "logarithm" intuition: $\mathcal{C}(1, x^a) \cong \mathcal{C}(a, x)$ (DaoFP §9.8). Internal homs make $\mathcal{C}$ self-[[Enriched Category|enriched]] (DaoFP §20.1). [[Defunctionalization]] approximates $b^a$ by a solution set of environments.
- Generalization: [[Monoidal Closed Category]] ($[a, b]$ right adjoint to $- \otimes a$); [[Compact Closed Category]] (duals $a^*$ with $[a, b] \cong a^* \otimes b$).

````tabs
tab: Julia
```julia
# Julia functions are values: the exponential object is the (abstract) function type
curry(f) = x -> a -> f((x, a))
uncurry(h) = ((x, a),) -> h(x)(a)
apply(fa) = fa[1](fa[2])                     # evaluation ε : b^a × a → b
plus = ((x, a),) -> x + a
p = curry(plus); p(3)(4)                     # 7  (7 Sketches Exercise 3.73: p(3) = "add three")

# Catlab: FinSet is cartesian closed; the exponential of finite sets
using Catlab
# (not a built-in constructor in 0.16; |C^B| = |C|^|B|)
```
tab: Lean
```lean
#check CategoryTheory.exp                  -- exp A : C ⥤ C, A ⟹ B (notation) in a cartesian closed category
#check CategoryTheory.exp.adjunction       -- (prod.functor.obj A) ⊣ (exp A)
#check CategoryTheory.CartesianClosed.curry
#check CategoryTheory.CartesianClosed.uncurry
#check CategoryTheory.exp.ev               -- evaluation A ⨯ (A ⟹ B) ⟶ B
example : CategoryTheory.CartesianClosed (Type u) := inferInstance
```
tab: Haskell
```haskell
-- DaoFP Chapter 6: the function type is the exponential object
apply :: (a -> b, a) -> b            -- elimination rule / evaluation / modus ponens
apply (f, x) = f x
apply' = uncurry id                  -- the Yoneda trick: ε = α⁻¹(id)

curry' :: ((x, a) -> b) -> (x -> a -> b)      -- introduction rule
curry' f x a = f (x, a)
uncurry' :: (x -> a -> b) -> ((x, a) -> b)
uncurry' h (x, a) = h x a

-- functoriality (the function type is a profunctor)
dimap :: (a' -> a) -> (b -> b') -> (a -> b) -> (a' -> b')
dimap f g h = g . h . f
```
````
