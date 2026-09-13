#definition #example

An arrow $f : a \to b$ in a [[Category]] is a **monomorphism** ("mono", drawn $a \hookrightarrow b$ or $a \rightarrowtail b$) if for every object $c$ and every pair $g_1, g_2 : c \to a$,

$$
f \circ g_1 = f \circ g_2 \implies g_1 = g_2.
$$

Equivalently, post-composition $(f \circ -) : \mathcal{C}(c, a) \to \mathcal{C}(c, b)$ is injective for every $c$. To show $f$ is *not* mono, exhibit two different "shapes" in $a$ that $f$ maps to the same shape in $b$.

> Sources: DaoFP §2.4 ("Monomorphisms"), Exercise 2.4.1; Kittenlab Lecture 14 (subobjects as injections); 7 Sketches §7.2 (subobjects), §1.4.2 (epi-mono factorization).

**Motivation (DaoFP).** `injectBool :: Bool -> Int` (`True ↦ 1`, `False ↦ 0`) doesn't discard information — it embeds a two-element shape in the integers; `even :: Int -> Bool` does discard (it abstracts). In $\mathbf{Set}$, [[Injection|injective]] means $f \circ x_1 = f \circ x_2 \Rightarrow x_1 = x_2$ for [[Global Element|global elements]] $x_i : 1 \to a$; since not every category has a terminal object, monomorphisms replace global elements by arbitrary shapes $c$.

- In $\mathbf{Set}$, monos are exactly the injections. Any arrow *from* the [[Terminal Object]] is mono ([[DaoFP Exercise 2.4.1]]).
- "In category theory objects are indivisible, so we can only talk about sub-objects using arrows": a mono $a \hookrightarrow b$ picks a [[Subobject]] of $b$ in the shape of $a$; in a [[Topos]] subobjects are classified by the [[Subobject Classifier]].
- Mono + [[Epimorphism|epi]] does *not* imply [[Isomorphism]] in general (e.g. $\mathbb{Z} \hookrightarrow \mathbb{Q}$ in rings); it does in $\mathbf{Set}$ ([[Bijection]]). A [[Section and Retraction|section]] is always mono.
- Every function factors as an epi followed by a mono ([[Epi-Mono Factorization]]).
- **Via pullbacks** (7 Sketches Definition 7.5): $f : A \to B$ is mono iff the square with $\mathrm{id}_A$ twice on top/left and $f$ twice on right/bottom is a [[Pullback]] — i.e. the kernel pair of $f$ is trivial. From this, $\mathbf{Set}$-monos are the injections ([[7S Exercise 7.6]]), and monos are stable under pullback ([[7S Exercise 7.8]], via the [[Pasting Lemma for Pullbacks]]). In a [[Topos]] every mono is the pullback of $\mathsf{true} : 1 \to \Omega$ along its characteristic map ([[Subobject Classifier]]).

````tabs
tab: Julia
```julia
using Catlab
is_monic(FinFunction([1, 3], 3))      # true: injective
is_monic(FinFunction([1, 1], 3))      # false
```
tab: Lean
```lean
#check CategoryTheory.Mono           -- class Mono f : ∀ g h, g ≫ f = h ≫ f → g = h
#check @CategoryTheory.mono_iff_injective
#check @CategoryTheory.mono_comp
```
tab: Haskell
```haskell
injectBool :: Bool -> Int             -- a monomorphism in Hask
injectBool b = if b then 1 else 0

-- in Hask, mono-ness of f amounts to injectivity; on a finite domain:
isMono :: (Eq a, Eq b) => [a] -> (a -> b) -> Bool
isMono as f = and [ x == y | x <- as, y <- as, f x == f y ]
```
````
