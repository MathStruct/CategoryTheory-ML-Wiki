#definition #theorem #example #program

**Abstract syntax with binding** is the problem of representing terms with binders — $\lambda x.\, t$, $\forall x.\, \varphi$, `let x = … in …` — so that **α-equivalent terms are equal**: $\lambda x.\, x$ and $\lambda y.\, y$ differ only in a bound name, which no observation can see. A plain [[Initial Algebra]] of a [[Polynomial Functor]] gets this wrong, because names are data. Three categorical answers fix it by changing the category in which the initial algebra is taken:

1. **Presheaves over finite sets** (Fiore, Plotkin & Turi). Index terms by the set of variables in scope: $\Lambda(n)$ = terms with free variables among $\{0, \dots, n-1\}$. Then $\Lambda$ is a functor $\mathbb F \to \mathbf{Set}$ on finite sets (renaming is functoriality), and the λ-calculus is the initial algebra of
   $$
   \Lambda \;\cong\; V + \Lambda \times \Lambda + \delta\Lambda, \qquad (\delta\Lambda)(n) = \Lambda(n+1),
   $$
   where $V$ is the presheaf of variables and $\delta$ "adds one variable to the context". The resulting terms are **de Bruijn terms**; capture-avoiding substitution makes $\Lambda$ a monoid for a substitution tensor.
2. **Nominal sets** (Gabbay & Pitts). Keep names, but work in sets with an action of the group of name permutations, where every element has finite support. An abstraction $[\mathbb A]X$ identifies $\langle a \rangle x$ and $\langle b \rangle ((a\ b) \cdot x)$ when $b$ is fresh; the initial algebra of $\Lambda \cong \mathbb A + \Lambda \times \Lambda + [\mathbb A]\Lambda$ is λ-terms up to α. Structural induction with a "pick a fresh name" principle comes for free.
3. **Locally nameless.** Bound variables are de Bruijn indices, free variables are names. A pragmatic mix that most proof assistants and Sophia use.

> Sources: Fiore, Plotkin & Turi, *Abstract syntax and variable binding*, LICS 1999; Gabbay & Pitts, *A new approach to abstract syntax with variable binding*, Formal Aspects of Computing 13 (2002); de Bruijn, *Lambda calculus notation with nameless dummies*, Indag. Math. 34 (1972); Allais, Atkey, Chapman, McBride & McKinna, *A Type and Scope Safe Universe of Syntaxes with Binding: Their Semantics and Proofs* (extended version of the ICFP 2018 paper), [arXiv:2001.11001](https://arxiv.org/abs/2001.11001) ([[A Type and Scope Safe Universe of Syntaxes with Binding|notes]]) (generic descriptions of syntaxes with binding; renaming, substitution and printing derived once for all of them); Maziarz, Ellis, Lawrence, Fitzgibbon & Peyton Jones, *Hashing Modulo Alpha-Equivalence*, PLDI 2021, [arXiv:2105.02856](https://arxiv.org/abs/2105.02856) ([[Hashing Modulo Alpha-Equivalence|notes]]) §§2.3–2.5, Definition 6.4, Theorems 6.3, 6.7, 6.8.

## Scope safety and generic semantics

Indexing by context makes ill-scoped terms untypable: $\Lambda(n)$ simply contains no term mentioning variable $n$. Allais et al. push this to a **universe of syntaxes**: a datatype of *descriptions* of binding signatures, one generic term type over all of them, and generic traversals — renaming, substitution, printing, normalisation by evaluation, CPS — written and proved correct once, by a "semantics" record that says what to do at variables, binders and constructors. Each operation is an algebra; the traversal is the catamorphism. The same idea, without dependent types, is what a compiler framework needs when it supports many languages: write binding-aware operations once over a description of each language's binders.

## Hashing modulo α

A content-addressed store must give α-equivalent terms the same hash. For a *closed* term that is easy — hash its de Bruijn form. For *all subterms* at once (what common-subexpression elimination and maximal sharing need) it is not, and Maziarz et al. compare the options:

| hash each subterm by | cost | false positives | false negatives |
|---|---|---|---|
| its raw structure (names included) | $O(n)$ | yes | no |
| its de Bruijn form *in place* | $O(n \log n)$ | yes | yes |
| its locally nameless form *in isolation* | $O(n^2 \log n)$ | no | no |
| an **e-summary** (structure + variable map) | $O(n \log^2 n)$ | no | no |

De Bruijn indices of an open subterm depend on how many binders sit *above* it, so $\lambda y.\, x + y$ under one binder and under two get different forms (a false negative), while unrelated variables can collide (a false positive). Re-converting each subterm in isolation is correct but quadratic. Their algorithm computes, compositionally, an **e-summary** — the term's structure with variables erased, plus a map from free variables to their positions — which determines the term up to α exactly and can be hashed with weak (XOR) combiners in $O(n \log^2 n)$ (Theorem 6.3). With random hash functions (Definition 6.4) the collision probability is bounded explicitly (Theorems 6.7–6.8): 128-bit hashes suffice for billion-node expressions.

## Sophia

Sophia hashes definitions in **locally nameless** form after canonicalisation, which is correct; it also wants every subterm to be a content-addressed node, which is exactly the "all subterms" setting where locally nameless is quadratic. Maziarz et al.'s e-summary algorithm is the published fix, and their analysis also checks Sophia's choice of 256-bit hashes (a 128-bit display form is, by Theorem 6.8, safe as well). See [Hashing and Identity](https://mathstruct.org/Sophia/vault/Design/Hashing-and-Identity).

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# Named λ-terms: (:var, x), (:lam, x, body), (:app, f, a), (:add, a, b), (:const, c)
# de Bruijn form *relative to a context* of enclosing binders: bound variables become indices.
function db(t, env = Symbol[])
    k = t[1]
    k == :var   ? ((i = findfirst(==(t[2]), reverse(env))) === nothing ? (:free, t[2]) : (:bound, i - 1)) :
    k == :lam   ? (:lam, db(t[3], [env; t[2]])) :
    k == :const ? t : (k, db(t[2], env), db(t[3], env))
end
# α-equivalent closed terms get identical de Bruijn forms, hence identical hashes:
id1 = (:lam, :x, (:var, :x)); id2 = (:lam, :y, (:var, :y))
hash(db(id1)) == hash(db(id2))                                   # true
# Maziarz et al. §2.4: hashing *subterms* by their de Bruijn form inside the whole term fails.
# e = λx. foo (λy. x + y) (λz. λy. x + y): both arguments contain λy. x + y, α-equivalent as standalone terms.
inner = (:lam, :y, (:add, (:var, :x), (:var, :y)))
e = (:lam, :x, (:app, (:app, (:const, :foo), inner), (:lam, :z, inner)))
d = db(e)
s1 = d[2][2][3]; s2 = d[2][3][2]                              # the two occurrences, in place
(s1, s2)                     # λ. %1 + %0  vs  λ. %2 + %0: different, so de Bruijn misses the sharing
s1 == s2                     # false — a false negative for common-subexpression elimination
# Locally nameless: hash each subterm *in isolation*, free variables kept by name.
occ1 = e[3][2][3]; occ2 = e[3][3][3]                          # the two named occurrences of λy. x + y in e
db(occ1) == db(occ2)         # true: each in isolation is λ. x + %0, with x free by name
db(occ1)                     # (:lam, (:add, (:free, :x), (:bound, 0)))
```
tab: Lean
```lean
import Mathlib
-- Well-scoped λ-terms, indexed by the number of variables in scope (a presheaf on finite sets).
inductive Tm : Nat → Type
  | var {n : Nat} : Fin n → Tm n
  | app {n : Nat} : Tm n → Tm n → Tm n
  | lam {n : Nat} : Tm (n + 1) → Tm n

-- extend a renaming under a binder: the new variable 0 stays 0, the others shift
def liftRen {n m : Nat} (ρ : Fin n → Fin m) : Fin (n + 1) → Fin (m + 1) :=
  Fin.cases 0 (fun i => (ρ i).succ)

-- renaming is the functorial action of Tm on maps of finite sets
def Tm.rename : {n m : Nat} → (Fin n → Fin m) → Tm n → Tm m
  | _, _, ρ, .var i => .var (ρ i)
  | _, _, ρ, .app f a => .app (f.rename ρ) (a.rename ρ)
  | _, _, ρ, .lam b => .lam (b.rename (liftRen ρ))

-- α-equivalence is syntactic equality here: there are no bound names to rename
example : (Tm.lam (.var 0) : Tm 0) = Tm.lam (.var 0) := rfl
-- renaming by the identity does nothing on a closed term
example : (Tm.lam (.var 0) : Tm 0).rename id = Tm.lam (.var 0) := by
  simp [Tm.rename, liftRen]
```
tab: Haskell
```haskell
-- Converting named terms to de Bruijn form decides α-equivalence of closed terms.
import Data.List (elemIndex)

data Named = Var String | Lam String Named | App Named Named deriving Show
data DB = Bound Int | Free String | DLam DB | DApp DB DB deriving (Eq, Show)

toDB :: [String] -> Named -> DB
toDB env (Var x)   = maybe (Free x) Bound (elemIndex x env)   -- env lists innermost binder first
toDB env (Lam x b) = DLam (toDB (x : env) b)
toDB env (App f a) = DApp (toDB env f) (toDB env a)

alphaEq :: Named -> Named -> Bool
alphaEq s t = toDB [] s == toDB [] t

main :: IO ()
main = print ( alphaEq (Lam "x" (Lam "y" (App (Var "x") (Var "y"))))
                       (Lam "a" (Lam "b" (App (Var "a") (Var "b"))))     -- True
             , alphaEq (Lam "x" (Lam "y" (Var "x"))) (Lam "x" (Lam "y" (Var "y"))) )   -- False
```
````
