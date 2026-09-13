#definition #example

For a [[Category]] $\mathcal{C}$ and object $c$, the **slice category** $\mathcal{C}/c$ (**over-category**; Kittenlab: **typed objects**) has objects pairs $(e, p : e \to c)$ and morphisms $(e, p) \to (e', p')$ the arrows $f : e \to e'$ with $p' \circ f = p$:

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
e \arrow[rr, "f"] \arrow[dr, "p"'] & & e' \arrow[dl, "p'"] \\
 & c &
\end{tikzcd}
\end{document}
```

It "describes how $c$ is seen from the perspective of its category: the totality of arrows pointing at $c$", turning individual arrows into objects. Dually the **coslice** $c/\mathcal{C}$ (**under-category**) has objects $(a, i : c \to a)$.

> Sources: DaoFP §8.1 ("Slice categories", "Coslice categories"), §10.6 ([[Comma Category]] generalizes slices); Kittenlab Lecture 13 ("Typed objects": $\mathbf{FinSet}/T$).

- **Typed sets** (Kittenlab): for a set $T$ of types, a $T$-typed set is $(A, t : A \to T)$ and morphisms preserve types — exactly $\mathbf{Set}/T$. Typed [[Graph|graphs]] and typed [[Petri Net|Petri nets]] (e.g. RegNets) live in slices of $\mathbf{Grph}$ and $\mathbf{Petri}$. [[Product|Products]] in $\mathcal{C}/T$ are [[Pullback|pullbacks]] over $T$ (the "typed product").
- If $\mathcal{C}$ has a [[Terminal Object]] $1$, the coslice $1/\mathcal{C}$ has as objects all [[Global Element|global elements]] of all objects; a morphism $f : a \to b$ maps elements of $a$ to elements of $b$ — this "justifies our intuition of types as sets of values" (DaoFP).
- $\mathcal{C}/c$ is the [[Comma Category]] $\mathrm{Id}_{\mathcal{C}} \downarrow c$; the [[Category of Elements]] of a presheaf is a slice of the presheaf category; [[Dependent Type|fibrations]] and [[Dependent Type|dependent types]] are families in $\mathcal{C}/c$ (DaoFP Ch. 11: "type families as fibrations", base change $f^* : \mathcal{C}/c \to \mathcal{C}/c'$ by pullback, with adjoints $\Sigma_f \dashv f^* \dashv \Pi_f$).

````tabs
tab: Julia
```julia
# Kittenlab Lecture 13: a T-typed finite set is a FinFunction into T; morphisms commute over T
using Catlab
T = FinSet(2)                                   # two types
A = FinFunction([1, 2, 2], T); A′ = FinFunction([2, 1], T)
f = FinFunction([2, 1, 1], 2)                   # A → A′ as sets
compose(f, A′) == A                             # true: f preserves types, so it is a morphism of FinSet/T
# Catlab: the slice category and typed ACSets (e.g. typed Petri nets via `SliceCat` / `Slice`)
```
tab: Lean
```lean
#check CategoryTheory.Over        -- Over X : the slice C/X (a comma category)
#check CategoryTheory.Under       -- Under X : the coslice X/C
#check CategoryTheory.Over.mk
```
tab: Haskell
```haskell
-- an object of Hask/T is a type with a "typing" map into T
data Over t a = Over (a -> t)
-- a morphism (Over p) -> (Over p') is f :: a -> a' with p' . f = p (unenforced)
```
````
