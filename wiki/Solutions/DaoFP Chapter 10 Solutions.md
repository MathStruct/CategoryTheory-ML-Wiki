#solution

Solutions to the exercises of DaoFP, Chapter 10: [[DaoFP Chapter 10 Exercises]]. Index: [[Map of Content]].

## Solution 10.3.1

[[DaoFP Chapter 10 Exercises#Exercise 10.3.1|Exercise 10.3.1]]

For $f : x' \to x$: $\phi_{x',y} \circ (- \circ Lf) = (- \circ f) \circ \phi_{x,y}$ as maps $\mathcal{D}(Lx, y) \to \mathcal{C}(x', Ry)$ — pre-composition with $Lf$ upstairs corresponds to pre-composition with $f$ downstairs. See [[Adjunction]].

> Sources: DaoFP Exercise 10.3.1.

## Solution 10.3.2

[[DaoFP Chapter 10 Exercises#Exercise 10.3.2|Exercise 10.3.2]]

For $y \mapsto \mathcal{C}(x, Ry)$: $\mathcal{D}(Lx, y) \cong \mathcal{C}(x, Ry)$ naturally in $y$. See [[Representable Functor]].

> Sources: DaoFP Exercise 10.3.2.

## Solution 10.3.3

[[DaoFP Chapter 10 Exercises#Exercise 10.3.3|Exercise 10.3.3]]

$x \mapsto \mathcal{D}(Lx, y)$: $\mathcal{C}(x, Ry) \cong \mathcal{D}(Lx, y)$ naturally in $x$.

> Sources: DaoFP Exercise 10.3.3.

## Solution 10.5.1

[[DaoFP Chapter 10 Exercises#Exercise 10.5.1|Exercise 10.5.1]]

Counit of $(+) \dashv \Delta$: substitute $(a, b) := \Delta x = (x, x)$ in $\mathcal{C}(a + b, x) \cong (\mathcal{C} \times \mathcal{C})((a, b), \Delta x)$ and take the identity on the right: $\varepsilon_x = [\mathrm{id}_x, \mathrm{id}_x] : x + x \to x$, the codiagonal. Unit of $\Delta \dashv (\times)$: $\eta_x = \langle \mathrm{id}_x, \mathrm{id}_x \rangle : x \to x \times x$, the diagonal. See [[Unit and Counit of an Adjunction]].

> Sources: DaoFP Exercise 10.5.1.

## Solution 10.5.2

[[DaoFP Chapter 10 Exercises#Exercise 10.5.2|Exercise 10.5.2]]

$x \xrightarrow{\eta_x} R(Lx) \xrightarrow{Rg} Ry$: the mate of $g$ is $Rg \circ \eta_x$.

> Sources: DaoFP Exercise 10.5.2.

## Solution 10.5.3

#program — [[DaoFP Chapter 10 Exercises#Exercise 10.5.3|Exercise 10.5.3]]

`triangle = counit . fmap unit`: `fmap unit (L (2,'a')) = L (R (\r -> L (2, r)), 'a')`, then `counit` applies the function to `'a'`, giving `L (2, 'a')` — the identity, as required.

> Sources: DaoFP Exercise 10.5.3.

## Solution 10.5.4

#program — [[DaoFP Chapter 10 Exercises#Exercise 10.5.4|Exercise 10.5.4]]

The result is a function, so call it: `let R f = triangle' (R (+1)) in f 5` gives `6`, agreeing with `(+1) 5`. `unit (R g) = R (\r -> L (R g, r))`, and `fmap counit` turns each `L (R g, r)` into `g r`.

> Sources: DaoFP Exercise 10.5.4.

## Solution 10.9.1

[[DaoFP Chapter 10 Exercises#Exercise 10.9.1|Exercise 10.9.1]]

Unit $\eta_X : X \to U F X$, $x \mapsto [x]$ (singleton string). Counit $\varepsilon_m : F U m \to m$, evaluating a string of elements of $m$ by multiplying them (`foldr mappend mempty`, i.e. `mconcat`). See [[Free-Forgetful Adjunction]], [[List Monad]].

> Sources: DaoFP Exercise 10.9.1.

## Solution 10.9.2

#program — [[DaoFP Chapter 10 Exercises#Exercise 10.9.2|Exercise 10.9.2]]

```haskell
import Data.Monoid (Sum(..), Product(..))
sumL, prodL :: [Int] -> Int
sumL  = getSum     . foldMap Sum
prodL = getProduct . foldMap Product
-- sumL [1,2,3,4] == 10, prodL [1,2,3,4] == 24
```
The same "program" (the list) run by two interpreters. See [[Free Monoid]].

> Sources: DaoFP Exercise 10.9.2.
