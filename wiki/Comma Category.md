#definition #example

For a [[Functor]] $L : \mathcal{D} \to \mathcal{C}$ and an object $c \in \mathcal{C}$, the **comma category** $L \downarrow c$ (DaoFP: $L/c$) has objects pairs $(d, f : Ld \to c)$ and morphisms $(d, f) \to (d', f')$ the arrows $h : d \to d'$ with $f' \circ Lh = f$:

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
Ld \arrow[rr, "Lh"] \arrow[dr, "f"'] & & Ld' \arrow[dl, "f'"] \\
 & c &
\end{tikzcd}
\end{document}
```

It "describes the view of $c$ from the narrower perspective defined by the functor $L$" — think of $L$ as a model of $\mathcal{D}$ inside $\mathcal{C}$. Dually $c \downarrow R$ has objects $(d, f : c \to Rd)$. The general comma category $F \downarrow G$ of two functors into a common category has objects $(a, b, f : Fa \to Gb)$.

> Sources: DaoFP §10.6 ("Comma category", "Universal arrow"), §10.8 (Freyd's theorem: the comma category $L/c$ as a [[Cocone|cocone]] in $\mathcal{C}$ whose base is projected back to $\mathcal{D}$), Exercise 10.3.2; the [[Slice Category]] $\mathcal{C}/c$ is $\mathrm{Id} \downarrow c$; the [[Category of Elements]] of a presheaf is a comma category.

- A [[Universal Arrow]] from $L$ to $c$ is a [[Terminal Object]] in $L \downarrow c$; $L$ has a right adjoint iff every $L \downarrow c$ has a terminal object, and then $Rc$ is its underlying object and $\varepsilon_c$ its arrow ([[Adjunction]] via universal arrows).
- In the [[Adjoint Functor Theorem]] a **solution set** is a weakly terminal *set* in $L \downarrow c$; in [[Defunctionalization]], for $L = (- \times a)$ the comma category $L \downarrow b$ has objects (environment $e$, function $e \times a \to b$) and morphisms "reduce the environment".
- In the preorder case ([[Adjoint Functor Theorem for Preorders]]), $L \downarrow c$ is the set $\{d \mid L d \leq c\}$ and its "colimit" is the join defining the right adjoint.

````tabs
tab: Lean
```lean
#check CategoryTheory.Comma           -- Comma L R for L : A ⥤ T, R : B ⥤ T
#check CategoryTheory.CostructuredArrow   -- L ↓ c : objects (d, Ld ⟶ c)
#check CategoryTheory.StructuredArrow     -- c ↓ R : objects (d, c ⟶ Rd)
```
tab: Haskell
```haskell
-- DaoFP §10.8: an object of the comma category (-×a) ↓ b is an environment with a function out of it
data Comma a b e = Comma e ((e, a) -> b)
-- a morphism (Comma e f) -> (Comma e' f') is h :: e -> e' with f' . first h = f  (reduces the environment)
```
````
