#definition #example #program

A **signal flow graph** (Shannon, 1940s) is a [[Wiring Diagram]] whose dangling left wires are inputs, right wires outputs, built from icons over a [[Rig]] $R$ of signals:

| generator | icon | meaning | matrix | arity |
|---|---|---|---|---|
| amplify by $a \in R$ | box $a$ | multiply the signal by $a$ | $(a)$ | $1 \to 1$ |
| copy | black dot, 1 in 2 out | two copies of the input | $(1\ 1)$ | $1 \to 2$ |
| discard | black dot, 1 in 0 out | output nothing | $()$ ($1 \times 0$) | $1 \to 0$ |
| add | white dot, 2 in 1 out | sum of the inputs | $\binom{1}{1}$ | $2 \to 1$ |
| zero | white dot, 0 in 1 out | output $0$ | $()$ ($0 \times 1$) | $0 \to 1$ |

Formally (Definition 5.45), with $G_R := \{\text{copy}, \text{discard}, \text{add}, \text{zero}\} \cup \{a \mid a \in R\}$ and $s, t$ counting dangling wires, a **simplified signal flow graph** is a morphism of the [[Free Prop]] $\mathbf{SFG}_R := \mathrm{Free}(G_R)$; boxes are drawn as icons rather than labelled boxes.

> Sources: 7 Sketches §5.1 (Eq. 5.1), §5.3.2–5.3.5 (Definition 5.45, Examples 5.44, 5.46, Exercises 5.43, 5.55), §5.4 ([[Graphical Linear Algebra]], feedback, control theory), §5.5; [Sob] Graphical Linear Algebra blog, [BE15], [Zan15].

**Example (Eq. 5.1).** With inputs $x$ (top) and $y$ (bottom) and icons "copy $x$", "amplify $y$ by 7", "add", "amplify by 5", "amplify by 2", "copy", "amplify by 3", "add": the outputs are $u = 15x$ and $v = 3x + 21y$. Outputs can be computed by tracing signals forward, or by **summing over paths**: the contribution of input $i$ to output $j$ is the sum over all paths from $i$ to $j$ of the product of amplifications along the path (no backwards traversal); there is one path top-to-top with amplification $5 \cdot 3 = 15$, and none bottom-to-top.

**Example 5.44.** Linear differential equations $\dot x + 3 \ddot y - 2z = 0$, $\ddot y + 5 \dot z = 0$ become, via the Laplace transform with $D$ = "differentiate", a signal flow graph over $\mathbb{R}[D]$ with amplifications $D, 3D^2, -2, D^2, 5D$.

## Semantics

Every signal flow graph denotes a matrix: the prop functor $S : \mathbf{SFG}_R \to \mathbf{Mat}(R)$ ([[Functorial Semantics]], Theorem 5.53) sends the generators to the matrices above, and $S(g)$ is the $m \times n$ matrix whose $(i,j)$ entry is the total amplification from input $i$ to output $j$ (Proposition 5.54, by induction on prop expressions: composition is matrix multiplication by distributivity, monoidal product reindexes). Two graphs have the same behaviour iff they denote the same matrix; e.g. copy-then-copy-left and copy-then-copy-right both give $(1\ 1\ 1)$ (Eq. 5.47, [[7S Exercise 5.55]]). Theorem 5.60 gives the complete set of graphical rewrite rules ([[Graphical Linear Algebra]]); [[Prop of Matrices|every matrix]] is represented by a four-layer normal form (copy/discard, scalars, permutation, add/zero).

## Feedback and the behavioural approach (§5.4.3)

Reversing the icons ($g^{\mathrm{op}} : n \to m$ for $g : m \to n$) and interpreting graphs by their **behaviour** $B(g) = \{(x, S(g)x)\} \subseteq R^m \times R^n$ — a relation, with $B(g^{\mathrm{op}})$ the transposed relation — gives (non-simplified) signal flow graphs $\mathbf{SFG}^+_R := \mathrm{Free}(G_R \sqcup G_R^{\mathrm{op}})$ with semantics in the prop $\mathbf{Rel}_R$ ([[Category of Relations]]). Reversed add is $\{(x, (y, z)) \mid x = y + z\}$, reversed copy is $\{((y,z), x) \mid x = y = z\}$ ([[7S Exercise 5.77]]); $g \mathbin{;} h^{\mathrm{op}}$ has behaviour $\{(x, y) \mid S(g)x = S(h)y\}$ — solution sets of linear equations ([[7S Exercise 5.82]]); kernels and images arise from zero-reverse and discard-reverse ([[7S Exercise 5.84]]). Cups and caps (copy-then-discard reversed) make $\mathbf{Rel}_R$ [[Compact Closed Category|compact closed]] with every $n$ self-dual (Theorem 5.87), allowing **feedback** loops: a cruise-control system over $\mathbb{R}[s, s^{-1}]$ with $v = \int \frac{1}{m} F + u + p \int (a u + b v)$ asks how to choose $a, b$ so that the behaviour approximates $\{(F, u, v) \mid u = v\}$ (Willems' behavioural approach [Wil07]).

````tabs
tab: Julia
```julia
# evaluate a simplified signal flow graph as a matrix via the semantics functor S (Theorem 5.53)
copyM(R)    = reshape([R.one, R.one], 1, 2)          # 1 → 2
discardM(R) = zeros(typeof(R.one), 1, 0)             # 1 → 0
addM(R)     = reshape([R.one, R.one], 2, 1)          # 2 → 1
zeroM(R)    = zeros(typeof(R.one), 0, 1)             # 0 → 1
scalarM(R, a) = reshape([a], 1, 1)
idM(R, n) = [i == j ? R.one : R.zero for i in 1:n, j in 1:n]
dsum(R, A, B) = [A fill(R.zero, size(A,1), size(B,2)); fill(R.zero, size(B,1), size(A,2)) B]   # f + g
# Eq. (5.1) over ℕ, as four action columns: (copy + 7) ; (id + add) ; (5 + 2) ; ... → S = [15 3; 0 21]
col1 = dsum(Nat, copyM(Nat), scalarM(Nat, 7))                 # 2 → 3
col2 = dsum(Nat, idM(Nat, 1), addM(Nat))                      # 3 → 2
col3 = dsum(Nat, scalarM(Nat, 5), scalarM(Nat, 2))            # 2 → 2
col4 = dsum(Nat, copyM(Nat), idM(Nat, 1))                     # 2 → 3
col5 = dsum(Nat, scalarM(Nat, 3), addM(Nat))                  # 3 → 2
S = foldl((A, B) -> matmul(Nat, A, B), [col1, col2, col3, col4, col5])   # [15 3; 0 21]

# Catlab: signal flow graphs live in Catlab.Programs / Catlab.Graphics (e.g. `@program`, `to_graphviz`)
```
tab: Lean
```lean
-- semantics of the generators as matrices over a semiring R
def copyM (R : Type) [Semiring R] : Matrix (Fin 1) (Fin 2) R := fun _ _ => 1
def addM  (R : Type) [Semiring R] : Matrix (Fin 2) (Fin 1) R := fun _ _ => 1
def scalarM {R : Type} [Semiring R] (a : R) : Matrix (Fin 1) (Fin 1) R := fun _ _ => a
-- S(g) for a composite is the product of the layer matrices (Matrix.mul), monoidal product is `Matrix.fromBlocks`
```
tab: Haskell
```haskell
-- a signal flow graph as a prop expression over the icon signature, evaluated to a matrix
data Icon r = Copy | Discard | Add | Zero | Scalar r
type M r = [[r]]
iconMatrix :: Rig r => Icon r -> M r
iconMatrix Copy       = [[one, one]]
iconMatrix Discard    = [[]]
iconMatrix Add        = [[one], [one]]
iconMatrix Zero       = []
iconMatrix (Scalar a) = [[a]]
-- eval :: Rig r => Expr (Icon r) -> M r   uses matrix multiplication for Seq and block sums for Par
```
````
