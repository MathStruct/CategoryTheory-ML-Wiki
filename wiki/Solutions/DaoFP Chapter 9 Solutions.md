#solution

Solutions to the exercises of DaoFP, Chapter 9: [[DaoFP Chapter 9 Exercises]]. Index: [[Map of Content]].

## Solution 9.3.1

#proof — [[DaoFP Chapter 9 Exercises#Exercise 9.3.1|Exercise 9.3.1]]

$\gamma_y \circ Ff = \beta_y \circ \alpha_y \circ Ff = \beta_y \circ Gf \circ \alpha_x = Hf \circ \beta_x \circ \alpha_x = Hf \circ \gamma_x$, using naturality of $\alpha$ then $\beta$. See [[Natural Transformation]].

> Sources: DaoFP Exercise 9.3.1.

## Solution 9.3.2

#program — [[DaoFP Chapter 9 Exercises#Exercise 9.3.2|Exercise 9.3.2]]

`safeHead . fmap reverse` and `fmap reverse . safeHead`, both of type `[[a]] -> Maybe [a]`; e.g. on `[[1,2],[3]]` both give `Just [2,1]`, on `[]` both give `Nothing`. Equal by naturality of `safeHead`.

> Sources: DaoFP Exercise 9.3.2.

## Solution 9.3.3

#program — [[DaoFP Chapter 9 Exercises#Exercise 9.3.3|Exercise 9.3.3]]

Both send `[[1,2],[],[3]]` to `[Just 3, Nothing, Just 1]`; equal by naturality of `reverse`.

> Sources: DaoFP Exercise 9.3.3.

## Solution 9.5.1

[[DaoFP Chapter 9 Exercises#Exercise 9.5.1|Exercise 9.5.1]]

A cone with apex $x$ is $c_1 : x \to D_1$, $c_2 : x \to D_2$ with $c_2 = Df \circ c_1$, so it is determined by $c_1$: $\mathrm{Lim}\,D \cong D_1$ with legs $\mathrm{id}$ and $Df$. Elements $1 \to \mathrm{Lim}\,D$ are elements of $D_1$.

> Sources: DaoFP Exercise 9.5.1.

## Solution 9.5.2

#program — [[DaoFP Chapter 9 Exercises#Exercise 9.5.2|Exercise 9.5.2]]

`q' x` is `True` iff `x` is odd; `h q' (q x)` returns `q' 0 = False` when `x` is even and `q' 1 = True` when odd. Equal for all `x`. See [[Coequalizer]].

> Sources: DaoFP Exercise 9.5.2.

## Solution 9.6.1

[[DaoFP Chapter 9 Exercises#Exercise 9.6.1|Exercise 9.6.1]]

If $F a = \varnothing$ there are no natural transformations $\mathcal{C}(a, -) \Rightarrow F$ either: $\alpha_a : \mathcal{C}(a, a) \to F a$ would have to send $\mathrm{id}_a$ somewhere. Both sides are empty, so the bijection holds. See [[Yoneda Lemma]].

> Sources: DaoFP Exercise 9.6.1.

## Solution 9.6.2

#proof — [[DaoFP Chapter 9 Exercises#Exercise 9.6.2|Exercise 9.6.2]]

For $f : x \to y$ and $h : a \to x$: $\alpha_y(f \circ h) = F(f \circ h)(p) = F f (F h (p)) = F f(\alpha_x(h))$ by functoriality of $F$; that is $\alpha_y \circ (f \circ -) = Ff \circ \alpha_x$.

> Sources: DaoFP Exercise 9.6.2.

## Solution 9.6.3

#proof — [[DaoFP Chapter 9 Exercises#Exercise 9.6.3|Exercise 9.6.3]]

Naturality for $h : a \to x$ applied to $\mathrm{id}_a$: $\alpha_x(\mathcal{C}(a, h)(\mathrm{id}_a)) = F h (\alpha_a(\mathrm{id}_a))$, i.e. $\alpha_x(h) = Fh(p)$ since $\mathcal{C}(a, h)$ is post-composition.

> Sources: DaoFP Exercise 9.6.3.

## Solution 9.8.1

[[DaoFP Chapter 9 Exercises#Exercise 9.8.1|Exercise 9.8.1]]

$\mathrm{Lim}\,D$ represents the presheaf $x \mapsto [\mathcal{J}, \mathcal{C}](\Delta_x, D)$ (cones with apex $x$); $\mathrm{Colim}\,D$ represents the co-presheaf $x \mapsto [\mathcal{J}, \mathcal{C}](D, \Delta_x)$ (cocones).

> Sources: DaoFP Exercise 9.8.1.

## Solution 9.8.2

#proof — [[DaoFP Chapter 9 Exercises#Exercise 9.8.2|Exercise 9.8.2]]

$F \cong \mathcal{C}(a, -)$ means each $\mathcal{C}(a, c)$ is a singleton, i.e. $a$ is initial; conversely if $0$ is initial, $\mathcal{C}(0, -) \cong F$ naturally.

> Sources: DaoFP Exercise 9.8.2.

## Solution 9.8.3

#program — [[DaoFP Chapter 9 Exercises#Exercise 9.8.3|Exercise 9.8.3]]

```haskell
instance Representable Pair where
  type Key Pair = Bool
  tabulate g = Pair (g True) (g False)
  index (Pair a b) = \k -> if k then a else b
```
$x \times x \cong x^{\mathbf{2}}$; see [[Representable Functor]].

> Sources: DaoFP Exercise 9.8.3.

## Solution 9.8.4

#program — [[DaoFP Chapter 9 Exercises#Exercise 9.8.4|Exercise 9.8.4]]

Yes, by the [[Initial Object]] (`Void`): $\mathcal{C}(0, x) \cong 1$ — "the logarithm of 1 is 0".
```haskell
instance Representable Unit where
  type Key Unit = Void
  tabulate _ = U
  index U = absurd
```

> Sources: DaoFP Exercise 9.8.4.

## Solution 9.8.5

[[DaoFP Chapter 9 Exercises#Exercise 9.8.5|Exercise 9.8.5]]

Yes: $[a] \cong \sum_{n \in \mathbb{N}} a^n$, a coproduct of the representables $a^{\underline{n}} = \mathcal{C}(\underline{n}, a)$, one for each length. See [[Representable Functor]], [[Free Monoid]].

> Sources: DaoFP Exercise 9.8.5.
