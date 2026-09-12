#definition #example

A **rig** (**semiring**; "a ring without negatives") is a tuple $(R, 0, +, 1, \ast)$ with

(a) $(R, +, 0)$ a commutative [[Monoid]];
(b) $(R, \ast, 1)$ a monoid (not necessarily commutative);
(c) distributivity: $a \ast (b + c) = a \ast b + a \ast c$ and $(a + b) \ast c = a \ast c + b \ast c$;
(d) $a \ast 0 = 0 = 0 \ast a$.

Signals can be *added* and *amplified*, and amplification distributes over addition; the possible amplifications form a rig.

> Sources: 7 Sketches §5.3.1 (Definition 5.36, Examples 5.37–5.42, Exercise 5.41), Example 5.73; §5.3–5.4 ([[Signal Flow Graph|signal flow graphs]] over a rig, [[Prop of Matrices|$\mathbf{Mat}(R)$]]); [Gla13].

**Examples.** $(\mathbb{N}, 0, +, 1, \ast)$; the [[Booleans]] $(\mathbb{B}, \mathsf{false}, \vee, \mathsf{true}, \wedge)$; any [[Quantale]] $(V, \bigvee\varnothing, \vee, I, \otimes)$ — in particular [[Cost]] gives the tropical (min, +) rig; the $n \times n$ matrices $\mathrm{Mat}_n(R)$ over any rig (generally noncommutative: in $\mathrm{Mat}_2(\mathbb{N})$, $\begin{pmatrix}0&1\\0&0\end{pmatrix}\begin{pmatrix}0&1\\1&0\end{pmatrix} \neq$ the reverse product, [[7S Exercise 5.41]]); any ring, e.g. $\mathbb{R}$; the polynomial rig $\mathbb{R}[s, s^{-1}]$ of control theory ($s$ = integration, $s^{-1}$ = differentiation). A rig is a [[Monoid Object]] in $(\mathbf{CMon}, \otimes, \mathbb{N})$ (Example 5.73). Matrix multiplication $\sum_b M(a,b) \ast N(b,c)$ makes sense over any rig — [[Matrix Multiplication in a Quantale]] is the case of a quantale.

````tabs
tab: Julia
```julia
# a rig as a Julia struct of operations; the tropical rig and the Booleans
struct Rig{T}; zero::T; plus::Function; one::T; times::Function; end
Nat  = Rig(0, +, 1, *)
Bool_ = Rig(false, |, true, &)
Trop = Rig(Inf, min, 0.0, +)                    # Cost as a rig
matmul(R::Rig, M, N) = [reduce(R.plus, (R.times(M[i,k], N[k,j]) for k in axes(M,2)); init=R.zero)
                         for i in axes(M,1), j in axes(N,2)]
```
tab: Lean
```lean
#check Semiring          -- Mathlib's rig: additive comm monoid + monoid + distributivity + zero laws
example : Semiring ℕ := inferInstance
example : Semiring Bool := inferInstance
#check Tropical          -- the tropical semiring (min, +)
example (n : ℕ) (R : Type) [Semiring R] : Semiring (Matrix (Fin n) (Fin n) R) := inferInstance
```
tab: Haskell
```haskell
-- a rig class (Data.Semiring-style)
class Rig r where
  zero, one :: r
  (<+>), (<.>) :: r -> r -> r
instance Rig Int  where zero = 0; one = 1; (<+>) = (+); (<.>) = (*)
instance Rig Bool where zero = False; one = True; (<+>) = (||); (<.>) = (&&)
newtype Trop = Trop Double
instance Rig Trop where
  zero = Trop (1/0); one = Trop 0
  Trop a <+> Trop b = Trop (min a b); Trop a <.> Trop b = Trop (a + b)
```
````
