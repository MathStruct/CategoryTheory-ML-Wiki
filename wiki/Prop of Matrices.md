#definition #theorem #proof #example

Let $R$ be a [[Rig]]. An $(m \times n)$-matrix with values in $R$ is a function $M : \underline{m} \times \underline{n} \to R$. The **prop of $R$-matrices** $\mathbf{Mat}(R)$ has morphisms $m \to n$ the $m \times n$ matrices, composition by matrix multiplication

$$
(M \mathbin{;} N)(a, c) := \sum_{b \in \underline{n}} M(a, b) \ast N(b, c) \qquad (5.48)
$$

(in the diagrammatic convention a row vector $v$ acts by $v \mathbin{;} A$, Remark 5.49), and monoidal product the **direct sum** $A + B = \begin{pmatrix} A & 0 \\ 0 & B \end{pmatrix}$ ([[7S Exercise 5.51]]). Any combination of multiplication and direct sum is an *interconnection* of matrices. For fixed $n$, $\mathrm{Mat}_n(R)$ is itself a rig (Example 5.40); the prop assembles all sizes into one structure.

> Sources: 7 Sketches §5.3.3–5.4.2 (Definition 5.50, Theorem 5.53, Proposition 5.54, 5.56, Theorem 5.60, Examples 5.61, 5.68, 5.70, Exercises 5.51, 5.55, 5.58, 5.59, 5.62, 5.63, 5.69); Kittenlab Lecture 3–4 ($\mathbf{Mat}$ with objects $\mathbb{N}$, morphisms $n \times m$ matrices; the functor $\mathsf{Fin} \to \mathbf{Mat}$), 11 (coequalizers in $\mathbf{Mat}$); Chapter 2 ([[Matrix Multiplication in a Quantale]] is the quantale case).

## Results

- **Theorem 5.53 ([[Functorial Semantics]]).** There is a prop functor $S : \mathbf{SFG}_R \to \mathbf{Mat}(R)$ sending each [[Signal Flow Graph|icon]] to its matrix; it exists by the universal property of [[Free Prop|free props]]. **Proposition 5.54**: $S(g)(i, j)$ is the total amplification from input $i$ to output $j$ — proved by induction over prop expressions: for $\alpha \mathbin{;} \beta$, every path from $i$ to $k$ passes through some middle port $j$ and distributivity gives $\sum_j \alpha(i,j) \ast \beta(j,k)$; for $\alpha + \beta$ no new paths appear, only reindexing.
- **Proposition 5.56 (fullness).** Every matrix is $S(g)$ for some signal flow graph: for $M = \begin{pmatrix} a & b \\ c & d \end{pmatrix}$ use four layers — (i) copies (and discards), (ii) scalars $a, b, c, d$, (iii) swaps and identities rearranging wires, (iv) adds (and zeros) — so that there is exactly one path from input $i$ to output $j$ carrying exactly the scalar $M(i,j)$ (Eq. 5.57; general case [[7S Exercise 5.59]]; examples [[7S Exercise 5.58]]).
- **Theorem 5.60 (presentation).** $\mathbf{Mat}(R)$ is isomorphic to the prop presented by the generators $G_R$ and the equations of [[Graphical Linear Algebra]]: copy is a cocommutative comonoid, add is a commutative monoid, they form a bialgebra, and scalars interact by $a \mathbin{;} b = ab$, copying/discarding scalars, $a + b$ via copy-then-add, and $0$ = discard-then-zero. Proof idea: the equations rewrite any expression to the four-layer normal form.
- $(1, \text{add}, \text{zero})$ is a commutative [[Monoid Object]] in $\mathbf{Mat}(R)$ and $(1, \text{copy}, \text{discard})$ a cocommutative comonoid (Examples 5.68, 5.70); the functor $U : \mathbf{Mat}(R) \to \mathbf{Set}$, $n \mapsto R^n$, is a [[Monoidal Functor]] carrying it to the additive monoid $(R, +, 0)$ ([[7S Exercise 5.69]]).
- Kittenlab: the functor $\mathsf{Fin} \to \mathbf{Mat}$ sends a function to its 0/1 matrix; the [[Coequalizer]] of $M, 0 : n \rightrightarrows m$ is $m - \mathrm{rank}(M)$ with an orthonormal complement as coequalizing matrix.

````tabs
tab: Julia
```julia
# Mat(R) over a rig, with composition = matrix product and monoidal product = direct sum
matmul(R::Rig, M, N) = [reduce(R.plus, (R.times(M[i,k], N[k,j]) for k in axes(M,2)); init=R.zero)
                         for i in axes(M,1), j in axes(N,2)]
dsum(R, A, B) = [A fill(R.zero, size(A,1), size(B,2)); fill(R.zero, size(B,1), size(A,2)) B]
A = [3 3 1; 2 0 4]; B = [2 5 6 1]
dsum(Nat, A, B)          # Exercise 5.51: a 3 × 7 block matrix

# Catlab: the theory of biproduct categories / matrices; Kittenlab's Mat as a Category{Int, Matrix}
struct MatC <: Category{Int, Matrix{Float64}} end
Categories.dom(::MatC, M) = size(M, 1); Categories.codom(::MatC, M) = size(M, 2)
Categories.compose(::MatC, M, N) = M * N          # row-vector convention: v ⋅ M ⋅ N
Categories.id(::MatC, n::Int) = Matrix{Float64}(I, n, n)
```
tab: Lean
```lean
#check Matrix.mul               -- composition in Mat(R)
#check Matrix.fromBlocks        -- direct sum: fromBlocks A 0 0 B
example (R : Type) [Semiring R] (m n : ℕ) : Type := Matrix (Fin m) (Fin n) R
-- Mathlib bundles Mat(R) for a ring as the category `Matrix`-valued... see `CategoryTheory.Mat_` and `ModuleCat`
#check CategoryTheory.Mat_
```
tab: Haskell
```haskell
type M r = [[r]]
matmul :: Rig r => M r -> M r -> M r
matmul a b = [ [ foldr (<+>) zero (zipWith (<.>) row col) | col <- cols ] | row <- a ]
  where cols = foldr (zipWith (:)) (repeat []) b
dsum :: Rig r => M r -> M r -> M r
dsum a b = [ row ++ replicate nb zero | row <- a ] ++ [ replicate na zero ++ row | row <- b ]
  where na = if null a then 0 else length (head a); nb = if null b then 0 else length (head b)
```
````
