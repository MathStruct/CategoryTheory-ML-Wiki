#definition #theorem #example

For a [[Monad]] $(T, \eta, \mu)$ on $\mathcal{C}$, the **Kleisli category** $\mathcal{C}_T$ has the same objects as $\mathcal{C}$; an arrow $a \rightsquigarrow b$ (a **Kleisli arrow**) is an arrow $a \to T b$ of $\mathcal{C}$ (in Haskell `a -> m b`). Composition is the "fish" $g \mathbin{<=<} f = \mu_c \circ T g \circ f$ and the identity on $a$ is $\eta_a$ (`return`). The monad laws *are* the category laws of $\mathcal{C}_T$.

> Sources: DaoFP §14.2 ("Composing Effects": "The category that we have just defined is called the Kleisli category"), §14.3, §15.5 ("Kleisli category": the Kleisli adjunction is initial among adjunctions generating $T$); 7 Sketches (implicit: [[Closure Operator]]).

- **Kleisli adjunction** $L_T \dashv R_T$: $L_T : \mathcal{C} \to \mathcal{C}_T$ is the identity on objects and sends $f : a \to b$ to $\eta_b \circ f$; $R_T : \mathcal{C}_T \to \mathcal{C}$ sends $a \mapsto T a$ and a Kleisli arrow $g : a \to T b$ to $\mu_b \circ T g$. The hom-set isomorphism $\mathcal{C}_T(L_T a, b) \cong \mathcal{C}(a, R_T b)$ is the identity on representatives, and $R_T L_T = T$ ([[Monads from Adjunctions]]).
- $\mathcal{C}_T$ is (isomorphic to) the full subcategory of the [[Eilenberg-Moore Category]] $\mathcal{C}^T$ on the *free* algebras $(T a, \mu_a)$ — "inside every Eilenberg–Moore category there is a smaller Kleisli category struggling to get out". (The image of a functor need not be a subcategory in general, but $F^T$ is injective on objects.) Among all adjunctions generating $T$, Kleisli is initial and Eilenberg–Moore terminal.
- Example: for `Maybe`, `g <=< f = \a -> case f a of Nothing -> Nothing; Just b -> g b`, `return = Just`; composition short-circuits on failure. For the [[Writer Monad]], Kleisli arrows accumulate logs; for the [[State Monad]], `get` and `set` are the basic Kleisli arrows from which all stateful computations are built. Every library monad "comes with its own library of predefined basic Kleisli arrows".

````tabs
tab: Julia
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
