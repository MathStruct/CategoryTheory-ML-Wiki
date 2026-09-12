#definition #theorem #annotation

In Martin-Löf dependent type theory, **equality** (the *identity type*) is itself a [[Dependent Type]]: for every type $A$ and pair of values $x, y : A$ there is a type $\mathrm{Id}_A(x, y)$, written $x =_A y$, whose elements are *proofs* that $x$ equals $y$ (Curry–Howard). $0 =_{\mathbb{N}} 0$ is a type, like `Int`; $1 =_{\mathbb{N}} 0$ is hopefully uninhabited.

- **Introduction rule**: the dependent function $\mathrm{refl}_A : \Pi_{x : A}\, \mathrm{Id}_A(x, x)$ — reflexivity, "$\forall x.\ x = x$". Applying $\mathrm{refl}_\mathbb{N}$ to $0$ proves $0 = 0$.
- **Elimination rule** (the *J-rule*): given a family $T(x, y, p)$ over $x, y : A$, $p : \mathrm{Id}(x, y)$ and a function $t : \Pi_{x : A}\, T(x, x, \mathrm{refl}(x))$ defined *on the diagonal*, there exists $f : \Pi_{x, y : A} \Pi_{p : \mathrm{Id}(x,y)}\, T(x, y, p)$ with the computation rule $f(x, x, \mathrm{refl}(x)) = t(x)$. There is no "step" as for [[Natural Numbers Object|induction]]: J extends $t$ from the diagonal to the whole $(x, y, p)$-space by fiat — "a leap of faith", comparable to parametricity (all polymorphic functions are natural) or analytic continuation.
- There is **no uniqueness ($\eta$) rule** for equality: there may be equality proofs not obtained from $\mathrm{refl}$. This weakening is what makes *homotopy type theory* (HoTT) interesting: proofs are paths, proofs of equality of proofs are homotopies, and the **univalence axiom** states $(A = B) \cong (A \cong B)$ — equality is equivalent to equivalence, resolving the tension between category theorists' preference for isomorphism and the convenience of substituting equals for equals.

> Sources: DaoFP §11.5 ("Equality": "Equational reasoning", "Equality vs isomorphism", "Equality types", "Introduction rule", "$\beta$-reduction and $\eta$-conversion", "Induction principle for natural numbers", "Equality elimination rule").

## Definitional vs propositional equality

*Definitional* equality is proved by rewriting — substituting equals for equals using the clauses of definitions, $\beta$-reduction `(\x -> x + x) 2 = 2 + 2` — which is the basis of equational reasoning about pure programs (impossible with side effects). E.g. with `add n Z = n; add n (S m) = S (add n m)` one computes `add (S Z) (S Z) = S (S Z)` and `equal (S (S Z)) (S (S Z)) = True` step by step. But `add Z n = n` for *all* `n` needs induction: a *propositional* equality requiring an actual proof term.

For products, the computation rule ($\beta$: `fst (x, y) = x`) and the uniqueness rule ($\eta$: `(fst p, snd p) = p`) both follow from the universal property in the categorical model; for $\mathbb{N}$ and for $\mathrm{Id}$ the $\eta$ rule is absent.

````tabs
tab: Lean
```lean
import Mathlib
#check @Eq                     -- the identity type: Eq a b, notation a = b
#check @Eq.refl                -- introduction: ∀ x, x = x
#check @Eq.rec                 -- the J-rule (elimination)
#check @Eq.subst               -- substituting equals for equals
example : (1 : ℕ) + 1 = 2 := rfl              -- definitional equality
theorem zero_add' (n : ℕ) : 0 + n = n := by   -- propositional: needs induction
  induction n with
  | zero => rfl
  | succ k ih => rw [Nat.add_succ, ih]
```
tab: Haskell
```haskell
-- Haskell cannot express proofs of equality, but supports equational reasoning:
data Nat = Z | S Nat
equal :: Nat -> Nat -> Bool
equal Z Z = True
equal (S m) (S n) = equal m n
equal _ _ = False
add :: Nat -> Nat -> Nat
add n Z = n
add n (S m) = S (add n m)
-- add (S Z) (S Z) = S (add (S Z) Z) = S (S Z)   by rewriting with the clauses
-- With GADTs one can encode a propositional equality type:
-- data a :~: b where Refl :: a :~: a     (Data.Type.Equality)
```
````
