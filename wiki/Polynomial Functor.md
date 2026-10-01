#definition #theorem #example #program

A **polynomial functor** $p : \mathbf{Set} \to \mathbf{Set}$ is a coproduct of [[Representable Functor|representables]]:

$$
p(X) \;\cong\; \sum_{i \in I} X^{\,p[i]}
$$

for a set $I$ of **positions** and, for each position, a set $p[i]$ of **directions** (Niu & Spivak, Definitions 2.1, 2.8). Read as syntax: a position is a constructor, its directions are its argument slots, and an element of $p(X)$ is one constructor applied to arguments taken from $X$. A *finitary* signature — operations with arities — is exactly a polynomial with finite direction sets, and its polynomial is the "counting polynomial" $\sum_{f} y^{\mathrm{ar}(f)}$; for `lit`, `neg`, `add` it is $1 + y + y^2$.

The general version lives over slices of a locally cartesian closed category (Gambino & Kock §1.4): a polynomial is a diagram $I \xleftarrow{s} B \xrightarrow{f} A \xrightarrow{t} J$, and its functor is the composite

$$
\mathcal E/I \xrightarrow{\;s^*\;} \mathcal E/B \xrightarrow{\;\Pi_f\;} \mathcal E/A \xrightarrow{\;\Sigma_t\;} \mathcal E/J,
$$

pullback, then [[Dependent Product]], then [[Dependent Sum]] — the three basic functors of a [[Locally Cartesian Closed Category]]. The multi-sorted version ($I$ and $J$ = sets of sorts) handles typed syntax: terms, types and patterns as separate sorts.

> Sources: Gambino & Kock, *Polynomial functors and polynomial monads*, Math. Proc. Cambridge Philos. Soc. 154 (2013), [arXiv:0906.4931](https://arxiv.org/abs/0906.4931) ([[Polynomial Functors and Polynomial Monads|notes]]) §§1.4, 1.12, 1.14–1.16, 1.22, 1.24, 2.12, 4.3 (W-types); Niu & Spivak, *Polynomial Functors: A Mathematical Theory of Interaction*, [arXiv:2312.00990](https://arxiv.org/abs/2312.00990) ([[Polynomial Functors - A Mathematical Theory of Interaction|notes]]) Definitions 2.1, 2.8, 3.1, 3.65, 4.1; Abbott, Altenkirch & Ghani, *Containers: constructing strictly positive types*, Theor. Comput. Sci. 342 (2005) (the type-theoretic version: shapes and positions). DaoFP Ch. 6 for algebraic data types as sums of products.

## Closure properties

Polynomial functors compose (Gambino–Kock 1.12: substitution of polynomials), and the class of polynomial functors is the *smallest* class containing pullbacks and their adjoints and closed under composition (1.14). They have a natural strength (1.15) and preserve connected limits — in particular pullbacks — so they are **cartesian** functors (1.16). On $\mathbf{Set}$ a functor is polynomial iff it preserves connected limits (1.22, compare [[Representable Functor|representability]] componentwise). Finite polynomials with cartesian maps between them even recover arithmetic: the skeleton of that category is the [[Lawvere Theory]] of commutative semirings (1.24).

## Initial algebras are W-types — and ASTs

A polynomial endofunctor on $\mathbf{Set}$ always has an [[Initial Algebra]]: the set of well-founded trees whose nodes are positions and whose children are indexed by directions. In type theory these are **W-types**; in programming they are the **abstract syntax trees** of a signature. The initial algebra is the least fixed point $\mu p$ ([[Least Fixed Point]], categorified), built by iterating $W_{n+1} = p(W_n)$ from $\emptyset$ — trees of depth at most $n$ — and the terminal coalgebra $\nu p$ consists of possibly infinite trees. A category "has W-types" if every polynomial endofunctor has an initial algebra (Gambino–Kock §4.3); every topos with a natural-numbers object does.

So "a frontend's AST type" and "a polynomial functor" are the same datum, and a *catamorphism* (fold) out of the AST — evaluation, pretty-printing, **hashing** — is the unique algebra map out of $\mu p$. A Merkle hash is the fold for the algebra $f(h_1, \dots, h_k) \mapsto H(\mathrm{tag}(f) \,\|\, h_1 \,\|\, \cdots \,\|\, h_k)$.

## Poly: lenses between polynomials

Natural transformations between polynomial functors are **dependent lenses** (Niu–Spivak Definition 3.1): a map $p \to q$ is a forward map on positions together with, for each position, a *backward* map on directions $q[\varphi(i)] \to p[i]$. This makes $\mathbf{Poly}$ a category of interfaces: positions are what a system shows, directions what it accepts. A Moore machine is a lens $Sy^S \to p$ (Definition 4.1), and the [[Lens|lenses]] and [[Optic|optics]] of the ML track are the special case $p = By^A$. Gambino–Kock's Theorem 2.12 gives the general form of strong natural transformations between polynomial functors as diagrams.

## Sophia

Every syntax Sophia stores — the core calculus, MLIR operations, LLVM instructions, each frontend's surface AST — is the initial algebra of a polynomial functor, multi-sorted where there are separate sorts. Hashing is a fold, so it is determined by an algebra on hashes; and the "multiple ASTs" of the design are multiple polynomial functors with translations between their initial algebras. See [Multi-AST Layering](https://mathstruct.org/Sophia/vault/Design/Multi-AST-Layering) and [Hashing and Identity](https://mathstruct.org/Sophia/vault/Design/Hashing-and-Identity).

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# A signature = a polynomial p(y) = Σ_{op} y^{arity(op)}: positions are operators, directions are argument slots.
sig = Dict(:lit => 0, :neg => 1, :add => 2)                 # p(y) = 1 + y + y²
# The functor P(X) = Σ_op X^{arity}: one constructor applied to arguments from X.
P(X) = [(op, args) for (op, n) in sig for args in Iterators.product(ntuple(_ -> X, n)...)]
X = [:a, :b, :c]
length(P(X))                                                 # 13 = 1 + 3 + 3²
pcount(n) = sum(n^k for k in values(sig))                    # the polynomial, evaluated at |X|
length(P(P(X))) == pcount(pcount(length(X)))                 # true: composing functors = substituting polynomials
# The initial algebra (W-type) of P is the set of finite trees — closed terms over the signature.
# Trees of depth ≤ d: W₀ = ∅, W_{d+1} = P(W_d), so |W_{d+1}| = p(|W_d|).
W(d) = d == 0 ? Any[] : P(W(d - 1))
[length(W(d)) for d in 0:3]                                  # [0, 1, 3, 13]: lit; neg(lit), add(lit,lit); …
all(length(W(d + 1)) == pcount(length(W(d))) for d in 0:3)  # true: |W_{d+1}| = p(|W_d|), so |W_4| = p(13) = 183
```
tab: Lean
```lean
import Mathlib
-- Mathlib's polynomial functors: `PFunctor` (shapes A, positions B a), with W-types as initial algebras.
#check PFunctor           -- structure: A : Type u, B : A → Type u
#check @PFunctor.Obj      -- P.Obj X = Σ a : P.A, (P.B a → X)
#check @PFunctor.W        -- the W-type: well-founded trees of P

-- the signature lit | neg x | add x y as a polynomial functor
def sig : PFunctor where
  A := Fin 3                                   -- three constructors
  B := fun i => Fin ([0, 1, 2].getD i.val 0)   -- arities 0, 1, 2
```
tab: Haskell
```haskell
-- The base functor of the signature lit | neg x | add x y, and its initial algebra (the AST).
data ExprF x = Lit Int | Neg x | Add x x deriving Show

instance Functor ExprF where
  fmap _ (Lit n)   = Lit n
  fmap f (Neg x)   = Neg (f x)
  fmap f (Add x y) = Add (f x) (f y)

newtype Fix f = Fix (f (Fix f))

cata :: Functor f => (f a -> a) -> Fix f -> a          -- the unique algebra map out of μF
cata alg (Fix t) = alg (fmap (cata alg) t)

eval :: ExprF Int -> Int
eval (Lit n) = n
eval (Neg x) = negate x
eval (Add x y) = x + y

main :: IO ()
main = print (cata eval (Fix (Add (Fix (Lit 2)) (Fix (Neg (Fix (Lit 5)))))))   -- -3
```
````
