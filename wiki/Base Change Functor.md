#definition #theorem #example

For an arrow $f : b \to a$ in a category with [[Pullback|pullbacks]], the **base-change functor** (pullback functor, substitution)

$$
f^* : \mathcal{C}/a \to \mathcal{C}/b
$$

sends a fibration $\langle e, p : e \to a \rangle$ to $\langle f^* e, f^* p \rangle$, the pullback of $p$ along $f$, and a fiber-preserving map $h : e' \to e$ to the unique $f^* h : f^* e' \to f^* e$ induced by the universal property ([[DaoFP Exercise 11.2.4]]). Note that $f^*$ runs *opposite* to $f$.

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
f^* e \arrow[r, "g"] \arrow[d, "f^* p"'] \arrow[dr, phantom, "\lrcorner", very near start] & e \arrow[d, "p"] \\
b \arrow[r, "f"'] & a
\end{tikzcd}
\end{document}
```

> Sources: DaoFP §11.2 ("Pullbacks", "Substitution", "Base-change functor"), Exercise 11.2.4, §11.3–11.4; 7 Sketches §7.2 (pullback of subobjects), §3.4 ([[Data Migration Functor|$\Delta_F$]] as pullback of instances); Kittenlab Lecture 14 (pullback of a subset is its preimage).

- **In $\mathbf{Set}$**: $f^* E = \{(x, e) \mid f(x) = p(e)\}$. Think of $f$ as cutting the base $B$ into *patches* $f^{-1}(y)$ (with $A$ an *atlas* of patch names): $f^* E$ plants a clone of the fiber $p^{-1}(y)$ over every point of the patch $f^{-1}(y)$. Countries $A$, cities $B$, languages $E$ fibered by country: $f^* E$ assigns each city its country's languages.
- If $a = 1$: $f^* E = B \times E$, the *trivial bundle*. A general bundle is locally a product — a sum over patches of (patch $\times$ fiber), the atlas idea of differential geometry (Möbius strip, Klein bottle).
- Pulling back along a [[Global Element|point]] $x : 1 \to b$ extracts the single [[Fiber]] over $x$.
- **Adjoints** (in a [[Locally Cartesian Closed Category]]): $\Sigma_f \dashv f^* \dashv \Pi_f$, i.e. $f_! \dashv f^* \dashv f_*$ — the [[Dependent Sum]] ($f_!(s, q) = (s, f \circ q)$) and [[Dependent Product]]. On subobjects this is $\exists_f \dashv f^{-1} \dashv \forall_f$ ([[Quantification]], [[Direct Image, Preimage, and Dual Image]]).
- Type-theoretically $f^*$ is **substitution**: from the family $T(y)$, $y : B$, form $T(f(x))$, $x : A$. Also called *weakening* when $f$ is a projection $\Gamma \times X \to \Gamma$.

````tabs
tab: Julia
```julia
using Catlab
# base change in FinSet: pull the bundle p : E → A back along f : B → A
p = FinFunction([1, 1, 2], 2)          # E = 3 over A = 2: fibers of size 2 and 1
f = FinFunction([1, 1, 1, 2], 2)       # B = 4: patch {1,2,3} ↦ 1, patch {4} ↦ 2
P = pullback(f, p)
f_star_E = ob(P)                       # FinSet(7) = 3·2 + 1·1
f_star_p = legs(P)[1]                  # the new projection f*E → B
[length(preimage(f_star_p, x)) for x in 1:4]   # [2, 2, 2, 1]: fibers replanted over patches
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
#check @CategoryTheory.Over.pullback          -- (f : X ⟶ Y) : Over Y ⥤ Over X
#check @CategoryTheory.Over.mapPullbackAdj     -- Over.map f ⊣ Over.pullback f   (Σ_f ⊣ f^*)
#check @CategoryTheory.Over.pullbackComp       -- (f ≫ g)^* ≅ g^* ⋙ f^*
```
tab: Haskell
```haskell
-- base change on finite "bundles" represented as association lists (element, base point)
type Bundle e b = [(e, b)]
baseChange :: Eq a => (b -> a) -> Bundle e a -> [b] -> Bundle (b, e) b
baseChange f bundle bs = [ ((x, e), x) | x <- bs, (e, y) <- bundle, f x == y ]
```
````
