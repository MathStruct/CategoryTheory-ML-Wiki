#definition

A **category** $\mathcal{C}$ consists of the following data:

1. A collection $\mathrm{ob}(\mathcal{C})$ of **objects**
2. For each pair of objects $X, Y \in \mathrm{ob}(\mathcal{C})$, a collection $\mathcal{C}(X, Y)$ of **morphisms** (or arrows) from $X$ to $Y$
3. For each object $X$, an **identity morphism** $\mathrm{id}_X \in \mathcal{C}(X, X)$
4. For each triple of objects $X, Y, Z$, a **composition operation** $$\circ_{X,Y,Z} : \mathcal{C}(Y, Z) \times \mathcal{C}(X, Y) \to \mathcal{C}(X, Z)$$ written $(g, f) \mapsto g \circ f$

subject to the following axioms:

**Associativity**: For all morphisms $f : X \to Y$, $g : Y \to Z$, $h : Z \to W$, $$(h \circ g) \circ f = h \circ (g \circ f)$$

**Identity**: For all morphisms $f : X \to Y$, $$f \circ \mathrm{id}_X = f = \mathrm{id}_Y \circ f$$

---

## Type-Theoretic Formulation

A category may be encoded as a [[dependent type]] with the following signature:

$$ \begin{align*} &\mathcal{C} : \mathrm{Type} \ &\mathrm{Hom} : \mathcal{C} \to \mathcal{C} \to \mathrm{Type} \ &\mathrm{id} : \prod_{X : \mathcal{C}} \mathrm{Hom}(X, X) \ &\circ : \prod_{X, Y, Z : \mathcal{C}} \mathrm{Hom}(Y, Z) \to \mathrm{Hom}(X, Y) \to \mathrm{Hom}(X, Z) \ &\mathrm{assoc} : \prod_{X, Y, Z, W : \mathcal{C}} \prod_{f, g, h} (h \circ g) \circ f =_{\mathrm{Hom}(X,W)} h \circ (g \circ f) \ &\mathrm{id_left} : \prod_{X, Y : \mathcal{C}} \prod_{f : \mathrm{Hom}(X, Y)} \mathrm{id}_Y \circ f =_{\mathrm{Hom}(X,Y)} f \ &\mathrm{id_right} : \prod_{X, Y : \mathcal{C}} \prod_{f : \mathrm{Hom}(X, Y)} f \circ \mathrm{id}_X =_{\mathrm{Hom}(X,Y)} f \end{align*} $$

---

## Commutative Diagram

The associativity and identity axioms are expressed by the commutativity of:

```tikz
The compilation fails please fix
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
X \arrow[r, "f"] \arrow[dr, "g \circ f"'] \arrow[ddr, "h \circ g \circ f"', bend right=20] & Y \arrow[d, "g"] \arrow[dr, "h \circ g", bend left=10] & \\
& Z \arrow[d, "h"] & \\
& W &
\end{tikzcd}
\end{document}
```

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
X \arrow[r, "f"] \arrow[dr, "f"'] \arrow[d, "\mathrm{id}_X"'] & Y \arrow[d, "\mathrm{id}_Y"] \\
X \arrow[r, "f"'] & Y
\end{tikzcd}
\end{document}
```

---

````tabs
tab: Julia
```julia
# Some Julia code (Catlab) if available.
```
tab: Lean
```lean
class Category (C : Type u) where
  Hom : C → C → Type v
  id : (X : C) → Hom X X
  comp : {X Y Z : C} → Hom Y Z → Hom X Y → Hom X Z
  id_comp : ∀ {X Y : C} (f : Hom X Y), comp (id Y) f = f
  comp_id : ∀ {X Y : C} (f : Hom X Y), comp f (id X) = f
  assoc : ∀ {W X Y Z : C} (f : Hom W X) (g : Hom X Y) (h : Hom Y Z),
    comp (comp h g) f = comp h (comp g f)
```
tab: Haskell
```haskell
class Category cat where
  id :: cat a a
  (.) :: cat b c -> cat a b -> cat a c
  
  -- Laws (not enforceable):
  -- id . f = f
  -- f . id = f
  -- (h . g) . f = h . (g . f)
```
````
