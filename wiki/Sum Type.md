#definition #example #program

A **sum type** $a + b$ is the [[Coproduct]] of two types, seen from the programmer's side: it is defined by two arrows, the **data constructors** $\mathsf{Left} : a \to a + b$ and $\mathsf{Right} : b \to a + b$ (the *introduction rule*), together with the *elimination rule*: a mapping out $h : a + b \to c$ is the same as a pair of arrows $f : a \to c$, $g : b \to c$, with $h \circ \mathsf{Left} = f$ and $h \circ \mathsf{Right} = g$ (the *computation rules*). In Haskell the sum is `Either a b`; the elimination rule is pattern matching.

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
a \arrow[dr, "\mathsf{Left}"] \arrow[ddr, "f"', bend right] & & b \arrow[dl, "\mathsf{Right}"'] \arrow[ddl, "g", bend left] \\
 & a + b \arrow[d, "h", dashed] & \\
 & c &
\end{tikzcd}
\end{document}
```

> Sources: DaoFP Chapter 4 ("Sum Types": §4.1 "Bool", §4.2 "Enumerations", §4.3 "Sum Types", "Maybe", "Logic", §4.4 "Cocartesian Categories"), §6.1 (`either`, `unEither`, `bimap`), §5.2 ("Duality"); 7 Sketches §6.2.2; Kittenlab Lecture 11 (coproducts of types in Julia).

## Instances

- **Bool** $= 1 + 1$: two constructors `True, False :: Bool` (arrows from $1$); a function `Bool -> A` is the same as a pair of elements of `A`, written `if b then x else y`. So there are $0$ functions $2 \to 0$, $1$ function $2 \to 1$ and $4$ functions $2 \to 2$ — the counts $0^2, 1^2, 2^2$ of [[Exponential Object|exponentials]] ([[DaoFP Exercise 4.1.1]]). See [[Booleans]].
- **Enumerations**: `data RGB = Red | Green | Blue` is $1 + 1 + 1$; a function out of it is a triple of elements, written by pattern matching or `case`; the wildcard `_` matches everything else. `Char`, `Int`, `Double` are (huge) enumerations; `Integer` is genuinely infinite.
- **Maybe** $= 1 + a$: `data Maybe a = Nothing | Just a`, isomorphic to `Either () a`; used for partial functions instead of exceptions.
- **Logic**: $A + B$ is disjunction; to prove $C$ from $A + B$ one must handle both cases — exactly the two arrows of the elimination rule (Curry–Howard).
- **Recursive sums**: [[Natural Numbers Object|$N$]] ($1 + N$) and [[List|lists]] ($1 + a \times L_a$).

## Cocartesian categories

A category with all binary sums and an [[Initial Object]] $0$ is **cocartesian**. Using the Yoneda trick (compare mappings *out* of both sides, naturally in the target) one shows $1 + 0 \cong 1$, $a + 0 \cong a$ ([[DaoFP Exercise 4.4.1]]), $a + b \cong b + a$ ([[DaoFP Exercise 4.4.2]], [[DaoFP Exercise 4.4.3]]) and $(a + b) + c \cong a + (b + c)$, and $+$ is functorial: $\langle f, g \rangle := [\mathsf{Left} \circ f, \mathsf{Right} \circ g] : a + b \to a' + b'$ preserves composition and identities ([[DaoFP Exercise 4.4.4]], [[DaoFP Exercise 4.4.5]]). Hence $(\mathcal{C}, +, 0)$ is a [[Symmetric Monoidal Category]] (same content as [[7S Exercise 6.18]]). "When a child learns addition we call it arithmetic. When a grownup learns addition we call it a cocartesian category." The dual notion is a [[Cartesian Category]].

````tabs
tab: Julia
```julia
using Catlab
# sums of finite sets are coproducts; copairing implements the elimination rule
A = FinSet(2); B = FinSet(3)
S = coproduct(A, B)                     # A + B = FinSet(5), with coproj1, coproj2 (Left, Right)
f = FinFunction([1, 1], 2); g = FinFunction([2, 2, 1], 2)
h = copair(S, f, g)                     # [f, g] : 5 → 2
compose(coproj1(S), h) == f             # computation rule

# Julia's own sum types are Union{} / tagged structs; pattern matching by dispatch
either(f, g, x::Union{Some, Nothing}) = x === nothing ? g() : f(something(x))
```
tab: Lean
```lean
import Mathlib
#check @Sum                    -- α ⊕ β with Sum.inl, Sum.inr
#check @Sum.elim               -- (α → γ) → (β → γ) → α ⊕ β → γ   (elimination rule)
#check @Sum.elim_inl           -- computation rule
#check @Equiv.sumComm          -- α ⊕ β ≃ β ⊕ α
#check @Equiv.sumAssoc
#check @Equiv.sumEmpty         -- α ⊕ Empty ≃ α
#check @Bool.rec               -- the recursor for 2 = 1 + 1
```
tab: Haskell
```haskell
data Either a b where          -- introduction rules
  Left  :: a -> Either a b
  Right :: b -> Either a b

either :: (a -> c) -> (b -> c) -> Either a b -> c    -- elimination rule
either f _ (Left x)  = f x
either _ g (Right y) = g y

unEither :: (Either a b -> c) -> (a -> c, b -> c)     -- the other direction of the bijection
unEither h = (h . Left, h . Right)

data Maybe a = Nothing | Just a                       -- 1 + a
notB :: Bool -> Bool
notB b = if b then False else True
```
````
