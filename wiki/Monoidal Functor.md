#definition #example

Let $(\mathcal{C}, I_{\mathcal{C}}, \otimes_{\mathcal{C}})$ and $(\mathcal{D}, I_{\mathcal{D}}, \otimes_{\mathcal{D}})$ be [[Symmetric Monoidal Category|symmetric monoidal categories]]. A **(lax) symmetric monoidal functor** $(F, \varphi)$ consists of

(i) a [[Functor]] $F : \mathcal{C} \to \mathcal{D}$;
(ii) a morphism $\varphi_I : I_{\mathcal{D}} \to F(I_{\mathcal{C}})$;
(iii) morphisms $\varphi_{c_1, c_2} : F(c_1) \otimes_{\mathcal{D}} F(c_2) \to F(c_1 \otimes_{\mathcal{C}} c_2)$, natural in $c_1, c_2$ (the **coherence maps**),

obeying bookkeeping axioms compatible with associators, unitors and symmetries. It is **strong** if the $\varphi$ are isomorphisms and **strict** if identities. The preorder version is a [[Monoidal Monotone Map]]; reversing the $\varphi$ gives *oplax* monoidal functors.

> Sources: 7 Sketches §6.4.1 (Rough Definition 6.68, Example 6.69, Exercise 6.70), Definition 2.41, Exercise 5.69; DaoFP §14.9 ("Monoidal Functors": lax monoidal functors, functorial strength, applicative functors, closed functors), §17.7 (applicatives as monoids under [[Day Convolution]]), §20.1 (change of enriching category).

## Examples

- **Power set** $\mathcal{P} : (\mathbf{Set}, 1, \times) \to (\mathbf{Set}, 1, \times)$ with $\mathcal{P}(f) = $ direct image, $\varphi_I(1) = \{1\}$, $\varphi_{S,T}(A, B) = A \times B$ (Example 6.69; naturality is [[7S Exercise 6.70]]).
- **Decoration functors** $(F, \varphi) : (\mathbf{FinSet}, \varnothing, +) \to (\mathbf{Set}, 1, \times)$, e.g. $\mathbf{Circ}$ sending a finite set of nodes to the set of circuits on it, with $\psi_{V,V'}$ the disjoint union of circuits (Eq. 6.81); these build [[Decorated Cospan|decorated cospan]] categories (Theorem 6.77). The constant functor $F(c) = \{\ast\}$ recovers plain cospans ([[7S Exercise 6.78]]).
- $U : \mathbf{Mat}(R) \to \mathbf{Set}$, $n \mapsto R^n$, strong monoidal since $R^m \times R^n \cong R^{m+n}$ ([[7S Exercise 5.69]]); monoidal functors carry [[Monoid Object|monoid objects]] to monoid objects.
- **Applicative functors** (DaoFP §14.9): a lax monoidal endofunctor of $\mathbf{Hask}$, `unit :: () -> f ()`, `(>*<) :: (f a, f b) -> f (a, b)`, equivalently `pure` and `<*>`; every [[Monad]] is applicative. Lax monoidal functors $[\mathcal{C}, \mathbf{Set}]$ are monoids for [[Day Convolution]].
- The [[Free Prop|semantics functor]] $S : \mathbf{SFG}_R \to \mathbf{Mat}(R)$ is a strict monoidal (prop) functor; [[Change of Base]] uses a monoidal functor between enriching categories; [[Operad|operad functors]] are the operadic analogue.
- **Functorial strength** $\sigma_{a,b} : a \otimes F b \to F(a \otimes b)$ (DaoFP §14.9, §20.2) is a related notion: in a closed category strong = enriched.

````tabs
tab: Julia
```julia
# Example 6.69: the power set as a lax monoidal functor (Set, 1, ×) → (Set, 1, ×)
powerset(S) = [Set(c) for c in Iterators.map(collect, Iterators.filter(_ -> true, subsets(collect(S))))]
image(f, A::Set) = Set(f(a) for a in A)                         # P on morphisms
φ(A::Set, B::Set) = Set((a, b) for a in A, b in B)               # φ_{S,T}(A, B) = A × B
# naturality (Exercise 6.70): φ(image(f, A), image(g, B)) == image(((a,b),) -> (f(a), g(b)), φ(A, B))
```
tab: Lean
```lean
#check CategoryTheory.LaxMonoidalFunctor      -- ε : 𝟙_ D ⟶ F.obj (𝟙_ C), μ : F.obj X ⊗ F.obj Y ⟶ F.obj (X ⊗ Y), coherence
#check CategoryTheory.MonoidalFunctor         -- strong: ε and μ isomorphisms
#check CategoryTheory.LaxBraidedFunctor       -- compatible with braidings/symmetry
```
tab: Haskell
```haskell
-- DaoFP §14.9: a lax monoidal endofunctor of Hask (= Applicative)
class Functor f => Monoidal f where
  unit  :: () -> f ()
  (>*<) :: (f a, f b) -> f (a, b)
-- equivalent to Applicative: pure x = fmap (const x) (unit ()); ff <*> fa = fmap (uncurry ($)) (ff >*< fa)

-- functorial strength (free in Hask)
strength :: Functor f => (a, f b) -> f (a, b)
strength (a, fb) = fmap (\b -> (a, b)) fb
```
````

## Lax monoidal functors in programming (DaoFP §14.9)

Monoidal functors map monoids to monoids, and only the *lax* data $\varphi_I : j \to F i$, $\varphi_{ab} : F a \oplus F b \to F(a \otimes b)$ is needed: a [[Monoid Object]] $(m, \mu, \eta)$ goes to $(F m, F\mu \circ \varphi_{mm}, F \eta \circ \varphi_I)$. For endofunctors preserving the cartesian product this is the Haskell class `class Monoidal f where unit :: f (); (>*<) :: f a -> f b -> f (a, b)` ([[DaoFP Exercise 14.9.1]]). In a [[Cartesian Closed Category]] lax monoidal endofunctors coincide with lax *closed* ones ($F(b^a) \to (F b)^{F a}$), i.e. [[Applicative Functor|applicative functors]]; every [[Functorial Strength|strong]] [[Monad]] is one. The category $\mathbf{MonCat}$ of monoidal categories and monoidal functors is a [[2-Category]].
