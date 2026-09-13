#definition

A **monoidal natural transformation** between (lax) [[Monoidal Functor|monoidal functors]] $(F, \varphi), (G, \psi) : \mathcal{C} \to \mathcal{D}$ is a [[Natural Transformation]] $\alpha : F \to G$ compatible with the coherence maps:
$$\alpha_{c_1 \otimes c_2} \circ \varphi_{c_1, c_2} = \psi_{c_1, c_2} \circ (\alpha_{c_1} \otimes \alpha_{c_2}), \qquad \alpha_I \circ \varphi_I = \psi_I .$$
Monoidal categories, monoidal functors and monoidal natural transformations form the [[2-Category]] $\mathbf{MonCat}$.

> Sources: DaoFP §14.9 ("The category of monoidal categories with monoidal functors as arrows is called MonCat. In fact it's a 2-category, since one can define structure-preserving natural transformations between monoidal functors"); 7 Sketches §6.4 (morphisms of [[Decorated Cospan|decoration functors]] induce hypergraph functors between decorated cospan categories), §4.4.

- Between [[Applicative Functor|applicative functors]] a monoidal natural transformation is an *applicative morphism*: `t :: forall a. f a -> g a` with `t (pure x) = pure x` and `t (u <*> v) = t u <*> t v`.
- Between [[Monoidal Monotone Map|monoidal monotone maps]] there is at most one, so the preorder version is trivial.
- A [[Natural Transformation]] between [[Functorial Semantics|functorial semantics]] of a [[Prop]] (symmetric monoidal functors $\mathcal{P} \to \mathbf{Set}$) is a homomorphism of models, e.g. a monoid homomorphism between models of the theory of monoids.

````tabs
tab: Haskell
```haskell
-- an applicative morphism = monoidal natural transformation between lax monoidal endofunctors
maybeToList :: Maybe a -> [a]                       -- t (pure x) = pure x ; t (u <*> v) = t u <*> t v
maybeToList Nothing  = []
maybeToList (Just a) = [a]
```
````
