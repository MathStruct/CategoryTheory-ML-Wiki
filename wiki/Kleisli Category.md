#definition #theorem #example

For a [[Monad]] $(T, \eta, \mu)$ on $\mathcal{C}$, the **Kleisli category** $\mathcal{C}_T$ has the same objects as $\mathcal{C}$; an arrow $a \rightsquigarrow b$ (a **Kleisli arrow**) is an arrow $a \to T b$ of $\mathcal{C}$ (in Haskell `a -> m b`). Composition is the "fish" $g \mathbin{<=<} f = \mu_c \circ T g \circ f$ and the identity on $a$ is $\eta_a$ (`return`). The monad laws *are* the category laws of $\mathcal{C}_T$.

> Sources: DaoFP §14.2 ("Composing Effects": "The category that we have just defined is called the Kleisli category"), §14.3, §15.5 ("Kleisli category": the Kleisli adjunction is initial among adjunctions generating $T$); 7 Sketches (implicit: [[Closure Operator]]); CTfS §5.3 (Definition 5.3.3.1, Examples 5.3.3.2–5.3.3.9, Remark 5.3.2.7), §5.3.4 (Kleisli database instances)

- **Kleisli adjunction** $L_T \dashv R_T$: $L_T : \mathcal{C} \to \mathcal{C}_T$ is the identity on objects and sends $f : a \to b$ to $\eta_b \circ f$; $R_T : \mathcal{C}_T \to \mathcal{C}$ sends $a \mapsto T a$ and a Kleisli arrow $g : a \to T b$ to $\mu_b \circ T g$. The hom-set isomorphism $\mathcal{C}_T(L_T a, b) \cong \mathcal{C}(a, R_T b)$ is the identity on representatives, and $R_T L_T = T$ ([[Monads from Adjunctions]]).
- $\mathcal{C}_T$ is (isomorphic to) the full subcategory of the [[Eilenberg-Moore Category]] $\mathcal{C}^T$ on the *free* algebras $(T a, \mu_a)$ — "inside every Eilenberg–Moore category there is a smaller Kleisli category struggling to get out". (The image of a functor need not be a subcategory in general, but $F^T$ is injective on objects.) Among all adjunctions generating $T$, Kleisli is initial and Eilenberg–Moore terminal.
- **Monads formalize context** (CTfS §5.3). Working in $\mathcal{C}_T$ lets us "write things in the functional way while holding the underlying context": for partial functions ($T X = X \sqcup \{\star\}$) every arrow may fail; for $T X = X^A$ with $A$ the set of experimenters, every arrow $X \to Y$ is really $X \times A \to Y$ and composites only feed one experiment's data into another *by the same experimenter* (CTfS Example 5.3.3.4); for the [[Power Set Monad]] Kleisli arrows are relations; for the [[Distribution Monad]] they are stochastic maps, and a Kleisli arrow $S \to S$ is a [[Markov Chain]]. Every ordinary function is a Kleisli arrow via $\eta$ (CTfS Remark 5.3.3.3), so no old way of doing business is lost. Even schema morphisms are Kleisli arrows — of the paths monad on $\mathbf{Grph}$: vertices go to vertices and arrows to *paths* (CTfS Remark 5.3.2.7, [[Categories and Schemas are Equivalent]]). Functors from a schema into $\mathcal{C}_T$ are [[Kleisli Instance|Kleisli database instances]].
- Example: for `Maybe`, `g <=< f = \a -> case f a of Nothing -> Nothing; Just b -> g b`, `return = Just`; composition short-circuits on failure. For the [[Writer Monad]], Kleisli arrows accumulate logs; for the [[State Monad]], `get` and `set` are the basic Kleisli arrows from which all stateful computations are built. Every library monad "comes with its own library of predefined basic Kleisli arrows".

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# Kleisli arrows for the Maybe monad (Union{Some,Nothing}) and their composition
fish(g, f) = a -> (b = f(a); b === nothing ? nothing : g(something(b)))
safesqrt(x) = x < 0 ? nothing : Some(sqrt(x))
recip(x) = x == 0 ? nothing : Some(1 / x)
h = fish(recip, safesqrt)          # a ↝ c
h(4.0), h(-1.0), h(0.0)            # (Some(0.5), nothing, nothing)
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
#check @CategoryTheory.Kleisli            -- Kleisli T : Type u (objects of C)
#check @CategoryTheory.Kleisli.instCategory
#check @CategoryTheory.Kleisli.adjunction -- L_T ⊣ R_T
```
tab: Haskell
```haskell
(<=<) :: Monad m => (b -> m c) -> (a -> m b) -> (a -> m c)
g <=< f = \a -> f a >>= g

instance Monad' Maybe where               -- Kleisli-style instance (DaoFP §14.2)
  g <=< f = \a -> case f a of
                    Nothing -> Nothing
                    Just b  -> g b
  return' = Just
```
````
