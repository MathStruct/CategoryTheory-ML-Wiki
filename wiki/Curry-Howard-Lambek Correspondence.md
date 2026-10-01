#theorem #definition #example #program

The **Curry–Howard–Lambek correspondence** identifies three things:

| logic | type theory | category theory |
|---|---|---|
| proposition | type | object |
| proof of $A \vdash B$ | term $x : A \vdash t : B$ | morphism $A \to B$ |
| conjunction $\wedge$, truth $\top$ | product type $A \times B$, unit | [[Product]], [[Terminal Object]] |
| implication $\Rightarrow$ | function type $A \to B$ | [[Exponential Object]] $B^A$ |
| disjunction $\vee$, falsity $\bot$ | sum type, empty type | [[Coproduct]], [[Initial Object]] |
| proof normalisation | β-reduction | the equations of the structure (e.g. $\mathrm{eval} \circ \langle \mathrm{curry}\,f \times \mathrm{id} \rangle = f$) |

Curry and Howard matched the first two columns (intuitionistic natural deduction and the simply typed λ-calculus); Lambek added the third: **the simply typed λ-calculus with products is the internal language of [[Cartesian Closed Category|cartesian closed categories]]**. Precisely, the λ-terms over a set of base types, modulo $\beta\eta$-equality, form the *free* CCC on those base types, and an interpretation of the base types in any CCC $\mathcal C$ extends uniquely to a CCC functor — the denotational semantics of the λ-calculus in $\mathcal C$.

> Sources: Lambek & Scott, *Introduction to Higher Order Categorical Logic* (CUP 1986), Part I (cartesian closed categories and λ-calculus); Crole, *Categories for Types* (CUP 1993); Castellan, Clairambault & Dybjer, *Categories with Families: Unityped, Simply Typed, and Dependently Typed*, [arXiv:1904.00827](https://arxiv.org/abs/1904.00827) ([[Categories with Families - Unityped, Simply Typed, and Dependently Typed|notes]]) Theorems 3–5 (cartesian categories and CCCs are equivalent to contextual simply typed cwfs, with and without λβη), Theorems 9–12 (the dependent version: locally cartesian closed categories); Elliott, *Compiling to categories*, Proc. ACM Program. Lang. 1 (ICFP 2017). DaoFP Ch. 4–6 and 9 for the programming side, 7 Sketches §7 for the topos-theoretic one.

## The translation, concretely

A term in context $x_1 : A_1, \dots, x_n : A_n \vdash t : B$ denotes a morphism $A_1 \times \dots \times A_n \to B$, defined by structural recursion:

- a variable is a projection; weakening (a variable one binder further out) is precomposition with $\pi_1$;
- a pair is $\langle f, g \rangle$, the projections are $\pi_1, \pi_2$;
- $\lambda x.\, t$ is $\mathrm{curry}(\llbracket t \rrbracket)$ — the transpose under the adjunction $(-) \times A \dashv (-)^A$ ([[Currying]]);
- application $t\,u$ is $\mathrm{eval} \circ \langle \llbracket t \rrbracket, \llbracket u \rrbracket \rangle$.

The result is **point-free**: no variables, only composition and the CCC operations. This is Elliott's "compiling to categories" — a compiler plugin that turns Haskell functions into these combinators and then interprets them in *any* CCC: functions, circuits, automatic differentiation ([[Cartesian Differential Category|differentiable maps]]), interval analysis, GPU kernels. The same program, many semantics — [[Functorial Semantics]].

## Extensions along the same line

| type theory | category |
|---|---|
| + sums | bicartesian closed category ([[Bicartesian Closed Category]]) |
| + dependent types | [[Locally Cartesian Closed Category]], categories with families ([[Category with Families]]) |
| + linear types | symmetric monoidal closed category ([[Linear-Non-Linear Adjunction]]) |
| + effects (call-by-value) | Kleisli category of a strong monad, [[Freyd Category]] |
| + higher-order logic | [[Topos]] ([[Internal Language of a Topos]]) |
| + regular logic only ($\wedge, \exists$) | [[Cartesian Bicategory|cartesian bicategory]] of relations ([[Conjunctive Query|conjunctive queries]]) |

Each row is a "triangle" — logic, calculus, category — of the same shape. Theorem 12 of Castellan et al. is a sobering data point from the dependent row: equality in the free LCCC on one object (extensional type theory) is **undecidable**, so a dependently typed system cannot decide every equation its categorical semantics validates.

## Sophia

Sophia's design starts from this correspondence: types, terms and proofs are *one* syntactic category, viewed three ways, so they are one node kind distinguished by role edges. Its Core Calculus is dependently typed, which puts it in the LCCC row — and Theorem 12 is the reason Sophia folds only *definitional* equality into hashes and stores the rest as witnessed propositional equalities. See [Multi-AST Layering](https://mathstruct.org/Sophia/vault/Design/Multi-AST-Layering) and [Core Calculus](https://mathstruct.org/Sophia/vault/Design/Core-Calculus).

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# Simply typed λ-terms (de Bruijn): (:var, i), (:lam, b), (:app, f, a), (:pair, a, b), (:fst, t), (:snd, t), (:const, c)
# Translate Γ ⊢ t : A into a morphism Γ → A of a cartesian closed category, built only from
# id, ∘, ⟨·,·⟩, π₁, π₂, curry and eval (contexts are nested pairs ((…, x₁), x₂)).
compose(g, f) = x -> g(f(x)); pairing(f, g) = x -> (f(x), g(x))
π1(p) = p[1]; π2(p) = p[2]
curry(f) = x -> (y -> f((x, y)));  ev(p) = p[1](p[2])
const_(c) = _ -> c
function ccc(t)
    k = t[1]
    k == :var   ? (t[2] == 0 ? π2 : compose(ccc((:var, t[2] - 1)), π1)) :   # weakening = precompose π₁
    k == :lam   ? curry(ccc(t[2])) :                                         # λ = currying
    k == :app   ? compose(ev, pairing(ccc(t[2]), ccc(t[3]))) :               # application = eval ∘ ⟨f, a⟩
    k == :pair  ? pairing(ccc(t[2]), ccc(t[3])) :
    k == :fst   ? compose(π1, ccc(t[2])) :
    k == :snd   ? compose(π2, ccc(t[2])) : const_(t[2])
end
# direct evaluation in an environment, for comparison
function evalt(t, env)
    k = t[1]
    k == :var ? env[end - t[2]] : k == :lam ? (x -> evalt(t[2], [env; x])) :
    k == :app ? evalt(t[2], env)(evalt(t[3], env)) : k == :pair ? (evalt(t[2], env), evalt(t[3], env)) :
    k == :fst ? evalt(t[2], env)[1] : k == :snd ? evalt(t[2], env)[2] : t[2]
end
# two closed programs: swap = λp. (snd p, fst p), and twice = λf. λx. f (f x) applied to (3·) and 2
swap = (:lam, (:pair, (:snd, (:var, 0)), (:fst, (:var, 0))))
ccc(swap)(())((1, "a"))                                      # ("a", 1)
twice = (:lam, (:lam, (:app, (:var, 1), (:app, (:var, 1), (:var, 0)))))   # λf. λx. f (f x)
prog = (:app, (:app, twice, (:const, x -> 3x)), (:const, 2))
ccc(prog)(()) == evalt(prog, []) == 18                       # true: the categorical semantics agrees
# β-reduction is sound: (λx. b) a and b[a/x] have the same morphism
redex = (:app, (:lam, (:pair, (:var, 0), (:var, 0))), (:const, 7))
ccc(redex)(()) == ccc((:pair, (:const, 7), (:const, 7)))(())   # true
```
tab: Lean
```lean
import Mathlib
open CategoryTheory
-- Curry–Howard: a proof of an implication is a function; the K and S combinators are proofs.
example (A B : Prop) : A → B → A := fun a _ => a
example (A B C : Prop) : (A → B → C) → (A → B) → A → C := fun f g a => f a (g a)
-- Lambek: Type is cartesian closed. (Mathlib now phrases CCCs as `CartesianMonoidalCategory` +
-- `MonoidalClosed`; the older `CartesianClosed` class is deprecated.)
example : MonoidalClosed Type := inferInstance
#check @MonoidalClosed.curry    -- (A ⊗ Y ⟶ X) → (Y ⟶ A ⟹ X): the λ of the internal language
-- in Type the adjunction (− × A) ⊣ (A ⇒ −) is the equivalence of function types
example (A B C : Type) : (A × B → C) ≃ (A → B → C) := Equiv.curry A B C
```
tab: Haskell
```haskell
-- Point-free CCC combinators for the function category; λ-terms translate into these.
curry' :: ((a, b) -> c) -> a -> b -> c
curry' f a b = f (a, b)

eval' :: (b -> c, b) -> c
eval' (f, x) = f x

pairing :: (x -> a) -> (x -> b) -> x -> (a, b)
pairing f g x = (f x, g x)

-- ⟦λp. (snd p, fst p)⟧ in the empty context () : curry (pairing (snd . snd) (fst . snd))
swapCCC :: () -> (a, b) -> (b, a)
swapCCC = curry' (pairing (snd . snd) (fst . snd))

main :: IO ()
main = print (swapCCC () (1 :: Int, "a"), eval' (swapCCC (), (True, 'x')))   -- (("a",1),('x',True))
```
````
