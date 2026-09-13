#definition #example #theorem

Let $\mathcal{E}$ be a category with finite [[Limit|limits]] ([[Pullback|pullbacks]] and a [[Terminal Object]] $1$). A **subobject classifier** is an object $\Omega \in \mathcal{E}$ together with a [[Monomorphism]] $\mathsf{true} : 1 \to \Omega$ such that for every mono $m : X \rightarrowtail Y$ there is a *unique* morphism $\ulcorner m \urcorner : Y \to \Omega$, the **characteristic map** of $m$, making the left square a pullback. Conversely every $p : Y \to \Omega$ — a [[Predicate]] on $Y$ — determines the [[Subobject]] $\{Y \mid p\}$ by pulling back $\mathsf{true}$ (right square).

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
X \arrow[r, "!"] \arrow[d, "m"'] \arrow[dr, phantom, "\lrcorner", very near start] & 1 \arrow[d, "\mathsf{true}"] & & \{Y \mid p\} \arrow[r, "!"] \arrow[d] \arrow[dr, phantom, "\lrcorner", very near start] & 1 \arrow[d, "\mathsf{true}"] \\
Y \arrow[r, "\ulcorner m \urcorner"'] & \Omega & & Y \arrow[r, "p"'] & \Omega
\end{tikzcd}
\end{document}
```

Slogan: *subobjects of $Y$ are classified by predicates on $Y$*: $\mathrm{Sub}(Y) \cong \mathcal{E}(Y, \Omega)$, naturally in $Y$. Equivalently, $\Omega$ [[Representable Functor|represents]] the subobject functor.

> Sources: 7 Sketches §7.2.2, Definition 7.12, Eq. (7.13)–(7.15), Exercises 7.16–7.17, §7.4.1 ("The subobject classifier $\Omega$ in a sheaf topos"), Eq. (7.50)–(7.51), Example 7.54; Kittenlab Lecture 14 (subsets as maps to Bool).

## Examples

- **$\mathbf{Set}$**: $\Omega = \mathbb{B} = \{\mathsf{true}, \mathsf{false}\}$ ([[Booleans]]), $\mathsf{true} : 1 \to \mathbb{B}$ picks $\mathsf{true}$. For $X \subseteq Y$, $\ulcorner m \urcorner(y) = \mathsf{true}$ iff $y \in X$; conversely $\{Y \mid p\} = \{y \in Y \mid p(y) = \mathsf{true}\}$ (Eq. 7.15). Compare [[Upper Sets Classified by Maps to Bool]] for preorders.
- **Sheaves on a space** $\mathbf{Shv}(X, \mathrm{Op})$: $\Omega(U) := \{U' \in \mathrm{Op} \mid U' \subseteq U\}$ with restriction $U' \mapsto U' \cap V$ for $V \subseteq U$ (Eqs. 7.50–7.51). It is a [[Sheaf]]: a matching family $V_i \subseteq U_i$ glues to $V = \bigcup_i V_i$, since $V \cap U_j = \bigcup_i (V_i \cap U_j) = \bigcup_i (V_j \cap U_i) = V_j$. The map $\mathsf{true} : 1 \to \Omega$ sends the unique section over $U$ to $U$ itself. **Upshot: truth values are open sets** — "property $P$ is true on the open subset $U$". On the one-point space this recovers $\mathbb{B}$ ([[7S Chapter 7 Exercises#Exercise 7.52|7S Exercise 7.52]]).
- **Graphs** ([[Topos of Graphs]]): $\Omega_{\mathbf{Grph}}$ has two vertices $0, V$ and five arrows; $\mathsf{true}$ sends the loop of the terminal graph to $(V, V; A)$.
- **Presheaves** on $\mathcal{C}$: $\Omega(c)$ is the set of sieves on $c$ (found via the [[Yoneda Lemma]]).

## Logic from $\Omega$

The logical connectives are characteristic maps of specific subobjects of $\Omega \times \Omega$ or $\Omega$ ([[Internal Logic of a Topos]]): $\wedge = \ulcorner(\mathsf{true}, \mathsf{true})\urcorner$, $\neg = \ulcorner \mathsf{false} \urcorner$, etc. [[Modality|Modalities]] are certain maps $\Omega \to \Omega$.

````tabs
tab: Julia
```julia
using Catlab
# Set: characteristic function of N ⊆ Z restricted to a finite window
Y = -5:5
χ = [y >= 0 for y in Y]                       # ⌜m⌝ : Y → Bool
Y[χ]                                          # {Y | χ} = 0:5, the subobject back again

# Grph: Catlab computes the subobject classifier of any C-set category
Ω, subobjs = subobject_classifier(Graph)
Ω                                             # Graph with V = 2, E = 5  (Example 7.54)
# classify the subgraph H ⊆ G by the unique hom G → Ω whose pullback along true is H
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
#check @CategoryTheory.Classifier              -- structure: Ω, truth : ⊤_ C ⟶ Ω, unique χ with pullback
#check @CategoryTheory.HasClassifier
#check @CategoryTheory.HasClassifier.χ         -- the characteristic map ⌜m⌝
-- in Type, Prop is the subobject classifier: subsets ↔ predicates
example (Y : Type) : Set Y ≃ (Y → Prop) := Equiv.refl _
```
tab: Haskell
```haskell
-- in finite Hask, Bool classifies subobjects: a mono X ↣ Y ⇝ its characteristic map
classify :: Eq y => [y] -> (y -> Bool)           -- ⌜m⌝ for the image of a mono
classify img = (`elem` img)
pullbackTrue :: [y] -> (y -> Bool) -> [y]          -- {Y | p}
pullbackTrue ys p = filter p ys
-- classify . pullbackTrue ys  ≡ id on predicates over ys; the other way gives back the subset
```
````
