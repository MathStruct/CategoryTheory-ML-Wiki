#definition #theorem #proof #program

The **initial algebra** of an [[Endofunctor]] $F$ is the [[Initial Object]] $(i, \iota)$ of the category of [[Algebra of an Endofunctor|$F$-algebras]]: for every algebra $(a, \alpha)$ there is a unique algebra morphism $f : i \to a$, the **catamorphism** ("banana brackets" $(\!|\alpha|\!)$, `cata alg`). Its carrier is the *least fixed point* $\mu F$.

**Lambek's lemma.** The structure map $\iota : F i \to i$ is an [[Isomorphism]]; hence $F i \cong i$ — $i$ is a *fixed point* of $F$.

*Proof.* $(F i, F \iota)$ is an algebra, so initiality gives $h : i \to F i$ with $h \circ \iota = F \iota \circ F h$. Then $\iota \circ h$ is an algebra morphism $(i, \iota) \to (i, \iota)$ (paste the square for $h$ with the trivially commuting square for $\iota$), and so is $\mathrm{id}_i$; by uniqueness $\iota \circ h = \mathrm{id}_i$. Then $h \circ \iota = F \iota \circ F h = F(\iota \circ h) = F(\mathrm{id}) = \mathrm{id}_{F i}$. $\blacksquare$

> Sources: DaoFP §12.2 ("Initial algebra"), §12.3 ("Lambek's Lemma and Fixed Points", "Fixed point in Haskell"), §12.4 ("Catamorphisms", "Examples", "Lists as initial algebras"), §12.5 ("Initial Algebra from Universality"), §12.6 ("Initial Algebra as a Colimit"), Exercise 12.5.1; §7 (natural numbers and lists); §15.3.

## Catamorphisms

Reading the defining square with $\iota^{-1}$ gives the recursive formula $f = \alpha \circ F f \circ \iota^{-1}$:
```haskell
cata :: Functor f => Algebra f a -> Fix f -> a
cata alg = alg . fmap (cata alg) . out
```
All recursion lives in `Fix` and `cata`; the client supplies only the non-recursive functor and algebra. Examples: `cata eval e9 = 9`, `cata pretty e9 = "2 + 3 + 4"`; an algebra with carrier `Int -> String` prints with indentation; `Fix Maybe` is the [[Natural Numbers Object]] and its catamorphism is `rec init step`; `Fix (ListF a)` is the [[List]] with catamorphism `foldr`; the algebra `revAlg :: Algebra (ListF a) ([a] -> [a])` accumulating closures reverses a list efficiently — the trick behind `foldl`.

## Fixed points in Haskell

```haskell
data Fix f where In :: f (Fix f) -> Fix f     -- In is ι; out (In x) = x is ι⁻¹
```
`Expr = ExprF Expr` unfolds to `data Expr = Val Int | Plus Expr Expr`. For the list functor in any [[Monoidal Category]] with coproducts, $F x = I + a \otimes x$, the fixed point is the "geometric series" $L_a = I + a + a \otimes a + \cdots$, made rigorous as a colimit ([[Free Monoid]]).

## Universality and the colimit construction

- **Mu**: by a Yoneda-style argument, $\mu F$ is also the type of all its catamorphisms: `data Mu f = Mu (forall a. Algebra f a -> a)`, with `cataMu alg (Mu h) = h alg` and `fromList` built by recursion ([[DaoFP Chapter 12 Exercises#Exercise 12.5.1|DaoFP Exercise 12.5.1]]).
- **Colimit of the $\omega$-chain**: $F 0$ is the type of *leaves* (`Maybe Void` has only `Nothing`), $F^n 0$ the trees of depth $\leq n$. The chain $0 \xrightarrow{¡} F 0 \xrightarrow{F ¡} F^2 0 \to \cdots$ has colimit $i = \mathrm{Colim}\, \Gamma$, and if $F$ preserves colimits of $\omega$-chains (true in $\mathbf{Set}$) then $F i \cong \mathrm{Colim}(F \Gamma) \cong i$: the cocone triangles identify the duplicate copies of a tree in $F^3 0$ and $F^4 0$. The catamorphism to $(a, \alpha)$ is induced by the cocone $f_0 = ¡$, $f_{n+1} = \alpha \circ F f_n$. This needs leaves: if $F 0 \cong 0$ the chain stays at $0$ and $\mu F = 0$ (e.g. the identity or stream functor, [[DaoFP Chapter 13 Exercises#Exercise 13.2.3|DaoFP Exercise 13.2.3]]).
- Dual: [[Terminal Coalgebra]] $\nu F$, the greatest fixed point; there is a canonical $\rho : \mu F \to \nu F$.

````tabs
tab: Julia
```julia
# Fix and cata for a functor given by fmap; ExprF as in [[Algebra of an Endofunctor]]
struct Fix; unfix; end                             # In :: f (Fix f) -> Fix f
out(x::Fix) = x.unfix
cata(alg, fmap) = x -> alg(fmap(cata(alg, fmap), out(x)))
val(n) = Fix(ValF{Any}(n)); plus(a, b) = Fix(PlusF{Any}(a, b))
e9 = plus(plus(val(2), val(3)), val(4))
cata(eval_alg, fmap)(e9)                           # 9
cata(pretty_alg, fmap)(e9)                         # "2 + 3 + 4"
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
#check @CategoryTheory.Endofunctor.Algebra.Initial.strInv     -- Lambek: the inverse of ι
#check @CategoryTheory.Endofunctor.Algebra.Initial.str_isIso  -- ι is an isomorphism
#check @CategoryTheory.Endofunctor.Algebra.Initial.left_inv
-- ℕ with [zero, succ] is the initial algebra of Option (= 1 + X) in Type
#check @Nat.rec
```
tab: Haskell
```haskell
newtype Fix f = In { out :: f (Fix f) }

cata :: Functor f => Algebra f a -> Fix f -> a
cata alg = alg . fmap (cata alg) . out

val :: Int -> Fix ExprF
val n = In (ValF n)
plus :: Fix ExprF -> Fix ExprF -> Fix ExprF
plus e1 e2 = In (PlusF e1 e2)
e9 :: Fix ExprF
e9 = plus (plus (val 2) (val 3)) (val 4)      -- cata eval e9 == 9

data ListF a x = NilF | ConsF a x deriving Functor
revAlg :: Algebra (ListF a) ([a] -> [a])      -- reverse via closures (the foldl trick)
revAlg NilF = id
revAlg (ConsF a f) = \as -> f (a : as)

data Mu f = Mu (forall a. Algebra f a -> a)   -- the initial algebra as its catamorphisms
cataMu :: Algebra f a -> Mu f -> a
cataMu alg (Mu h) = h alg
```
````
