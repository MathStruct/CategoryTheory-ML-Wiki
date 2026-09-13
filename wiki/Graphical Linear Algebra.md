#theorem #example #annotation

**Graphical linear algebra** (Sobociński, Bonchi, Zanasi, Baez–Erbele) does linear algebra with [[Signal Flow Graph|signal flow graphs]]: matrices, kernels, images, and linear relations are diagrams, and equations between them are proved by local graphical rewrites.

> Sources: 7 Sketches §5.4 (Theorem 5.60, Examples 5.61, Exercises 5.62–5.63, §5.4.3, Theorem 5.87), §5.5; [Sob] (Graphical Linear Algebra blog: determinants, eigenvectors, "division by zero"), [BE15], [Zan15], [BSZ14; BSZ15; BS17], [FSR16].

## The presentation of $\mathbf{Mat}(R)$ (Theorem 5.60)

Generators: copy $\bullet\!<$, discard $\bullet$, add $>\!\circ$, zero $\circ$, and scalars $a \in R$. Equations, for all $a, b \in R$:
1. **Cocommutative comonoid** (copy/discard): coassociativity, counitality, cocommutativity — the [[Discard and Copy Axioms|copy and discard]] structure.
2. **Commutative monoid** (add/zero): associativity, unitality, commutativity.
3. **Bialgebra** laws: copy after add = add after copies (with a swap), add-then-discard = discard both, copy-of-zero = two zeros, zero-then-discard = empty diagram.
4. **Scalars**: $a$ then $b$ equals $ab$; $1 = \mathrm{id}$; copying then amplifying both by $a$ equals amplifying then copying; discarding after $a$ = discarding; $a$ into add from both wires = add then $a$; zero then $a$ = zero; and copy-then-$(a, b)$-then-add $= a + b$, discard-then-zero $= 0$.

*Proof idea*: these suffice to rewrite any $G_R$-expression into the four-layer normal form of Proposition 5.56 (copies left, scalars middle, adds right) — details in [BE15; BS17].

**Soundness and completeness.** Two signal flow graphs represent the same matrix **iff** one can be rewritten into the other using these equations and the prop axioms: *sound* (rewriting preserves the matrix) and *complete* (equal matrices are inter-rewritable). Example 5.61: two graphs for $\binom{0}{6}$ (copy–2–3–add vs. discard/6) are transformed into each other in three steps; [[7S Chapter 5 Exercises#Exercise 5.62|7S Exercise 5.62]] does the matrices of [[7S Chapter 5 Exercises#Exercise 5.58|7S Exercise 5.58]]; [[7S Chapter 5 Exercises#Exercise 5.63|7S Exercise 5.63]] shows two graphs differ over $\mathbb{N}$ because only the equation "$0$ = discard-then-zero" can break a left-to-right path and no $0$ scalar can arise, while over $\mathbb{N}/3\mathbb{N}$ they can be simplified.

## Beyond matrices: linear relations and feedback

Interpreting a graph $g : m \to n$ by its **behaviour** $B(g) = \{(x, S(g)x)\} \subseteq R^m \times R^n$ and mirror-image icons $g^{\mathrm{op}}$ by the transposed relation embeds $\mathbf{Mat}(R)$ in the prop $\mathbf{Rel}_R$ of relations (Definition 5.79, composition by "there exists a middle $y$", Eq. 5.78). Behaviours are **linear relations** (subspaces): closed under $+$ and scalars, and closed under composition ([[7S Chapter 5 Exercises#Exercise 5.84|7S Exercise 5.84]], [[7S Chapter 5 Exercises#Exercise 5.85|7S Exercise 5.85]]), so linear relations form a sub-prop $\mathbf{LinRel}_R$. Solution sets $\{(x,y) \mid S(g)x = S(h)y\}$, kernels (compose with reversed zeros) and images (compose with reversed discards) are all diagrams. There is a sound and complete presentation of $\mathbf{LinRel}_R$ with generators $G_R \sqcup G_R^{\mathrm{op}}$, the equations of Theorem 5.60 plus a few more, some of which say $\mathbf{Rel}_R$ is [[Compact Closed Category|compact closed]] with every $n$ self-dual (Theorem 5.87): the cup is "reversed discard, then copy" with behaviour $\eta_1 = \{(0, (x, x))\}$ and the cap is "reversed copy, then discard" with behaviour $\varepsilon_1 = \{((x,x), 0)\}$ (Eq. 5.86). This gives **feedback** and a graphical treatment of control theory ([FSR16]).

The chapter's moral: props with presentations turn diagram manipulation into rigorous proof — "a sound and complete reasoning system" — and the separation of syntax from semantics by a functor ([[Functorial Semantics]]) is "perhaps the most significant idea".

````tabs
tab: Julia
```julia
# behaviours: a signal flow graph as a linear relation; kernel and image via reversed icons (Exercise 5.84)
using LinearAlgebra
S = [1.0 2 0; 0 0 1]                       # S(g) : 2 → 3 in the row-vector convention x ↦ x * S
kernel = nullspace(S')                     # {x | x*S = 0}: compose with reversed zeros
image  = S'                                # columns span {x*S}: compose with reversed discards
```
tab: Haskell
```haskell
-- a linear relation between finite-dimensional spaces as a list of generating pairs (x, y)
type LinRel = [([Double], [Double])]
-- composition: pairs (x, z) such that some y has (x, y) and (y, z); for subspaces this is a
-- projection of an intersection — implement with a linear-algebra library.
```
````
