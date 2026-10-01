#definition #theorem #example #program

A **logical relation** is a family of relations indexed by types, defined by induction on the type so that it is automatically compatible with the language's constructs. For two interpretations $\llbracket - \rrbracket_1, \llbracket - \rrbracket_2$ of a simply typed language:

$$
\begin{aligned}
R_{\iota} &\subseteq \llbracket \iota \rrbracket_1 \times \llbracket \iota \rrbracket_2 \quad \text{chosen at base types,}\\
f \; R_{A \to B} \; g &\iff \forall x, y.\; x \, R_A \, y \implies f(x) \; R_B \; g(y),\\
(a, b) \; R_{A \times B} \; (a', b') &\iff a \, R_A \, a' \;\wedge\; b \, R_B \, b'.
\end{aligned}
$$

The **fundamental lemma** says every well-typed term is related to itself: if $\vdash t : A$ then $\llbracket t \rrbracket_1 \; R_A \; \llbracket t \rrbracket_2$. Because the relation at function types is defined to *respect application*, it is a [[Congruence]] by construction, and the lemma is proved by induction on typing derivations — no quantification over all contexts. This is why logical relations are the standard tool for proving [[Contextual Equivalence|contextual equivalence]], normalisation, representation independence, compiler correctness and **parametricity**.

> Sources: Tait, *Intensional interpretations of functionals of finite type I*, J. Symb. Logic 32 (1967) (reducibility, the first logical relation); Plotkin, *Lambda-definability and logical relations*, Edinburgh memo SAI-RM-4 (1973); Statman, *Logical relations and the typed λ-calculus*, Inform. and Control 65 (1985); Reynolds, *Types, abstraction and parametric polymorphism*, IFIP 1983; Wadler, *Theorems for free!*, FPCA 1989; Mitchell & Scedrov, *Notes on sconing and relators*, CSL 1992 (logical relations as gluing); Sterling & Harper, *Logical Relations as Types: Proof-Relevant Parametricity for Program Modules*, J. ACM 68(6) (2021), [arXiv:2010.08599](https://arxiv.org/abs/2010.08599) ([[Logical Relations as Types - Proof-Relevant Parametricity for Program Modules|notes]]) Theorems 4.1, 5.18, 5.31, Construction 5.27, Corollary 5.32; Appel & McAllester, *An indexed model of recursive types for foundational proof-carrying code*, ACM TOPLAS 23(5) (2001) (step-indexing); Patterson & Ahmed, *Linking types for multi-language software: have your cake and eat it too*, SNAPL 2017 (relations across languages).

## Parametricity

Reynolds' **abstraction theorem** is the fundamental lemma for System F, with the relation at a type variable left arbitrary: a polymorphic term is related to itself at *every* relation instantiating its type variables. Instantiating with the graph of a function $g$ yields Wadler's **free theorems** — any $f : \forall \alpha.\, [\alpha] \to [\alpha]$ satisfies $f \circ \mathrm{map}\, g = \mathrm{map}\, g \circ f$, so it can only rearrange, drop or duplicate elements, never inspect them. In categorical terms, parametric polymorphic functions are [[Natural Transformation|natural transformations]] — and more: dinatural, and natural with respect to relations, not just functions.

**Representation independence** is the same theorem used the other way: two implementations of an abstract type are interchangeable if there is a relation between their representations that the operations respect. Sterling and Harper prove it for a calculus of ML-style modules: any client of a queue interface returns the same boolean for a list-based and a batched-queue implementation (Theorem 4.1), by exhibiting a simulation.

## Extensions for realistic languages

The plain definition breaks as soon as types are recursive (the relation at $\mu X.\, F X$ would refer to itself). **Step-indexed** logical relations (Appel–McAllester) index the relation by a number of remaining computation steps, making the definition well founded; **Kripke** logical relations index by a *world* describing the current heap and its invariants, for state; **biorthogonality** ($\top\top$-closure) handles control effects such as continuations and exceptions. Each is needed for one more feature of a real language.

## Logical relations are gluing

Categorically, a logical relation over two models $M_1, M_2$ of a type theory is a model in the category obtained by **gluing** — the comma category of predicates (or relations) over $M_1 \times M_2$ along a functor such as $\mathrm{Hom}(1, -)$ (Mitchell–Scedrov's *sconing*). The fundamental lemma is the statement that the syntax, being initial, maps uniquely into the glued model, and the projection back recovers the original interpretations. Sterling and Harper take this to its modern form: they glue **toposes** (Artin gluing / recollement, Theorem 5.18), obtaining a topos of *parametricity structures* (Construction 5.27) that is itself a model of a parametric type theory (Theorem 5.31). Inside it, a logical relation is just a *type* — "logical relations as types" — and a simulation between two implementations is a third implementation carrying the representation invariant.

## Across languages

For two different languages, the relation at base types is the substance: which Julia `Int64` corresponds to which C++ `int64_t`, which `String` to which `std::string`. Multi-language semantics (Matthews–Findler, Patterson–Ahmed) define such cross-language logical relations and prove that linking related components is safe.

## Sophia

Sophia's `observational` equivalence level needs exactly this: an equivalence that licenses substitution must be a congruence, and quantifying over all contexts is infeasible. A logical relation over the Core Calculus provides it — and for cross-language claims the base-type relation $R \subseteq \mathcal D_1 \times \mathcal D_2$ is the per-language-pair correspondence the design treats as part of the trusted computing base. See [Equivalence and Witnesses](https://mathstruct.org/Sophia/vault/Design/Equivalence-and-Witnesses) and [Trusted Computing Base](https://mathstruct.org/Sophia/vault/Design/Trusted-Computing-Base).

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# Two implementations of the integers: native Int, and "difference pairs" (a, b) meaning a − b.
# A logical relation is defined by induction on types:
R_int(n, p) = n == p[1] - p[2]                                   # at the base type: same number
R_fun(Rdom, Rcod, dom1, dom2) = (f, g) ->                        # at A → B: related inputs ↦ related outputs
    all(Rcod(f(x), g(y)) for x in dom1, y in dom2 if Rdom(x, y))
ints = -3:3; pairs = [(a, b) for a in 0:4 for b in 0:4]
inc1(n) = n + 1;            inc2(p) = (p[1] + 1, p[2])
neg1(n) = -n;               neg2(p) = (p[2], p[1])
R_fun(R_int, R_int, ints, pairs)(inc1, inc2)                     # true
R_fun(R_int, R_int, ints, pairs)(neg1, neg2)                     # true
# the relation is closed under composition (an instance of the fundamental lemma)
R_fun(R_int, R_int, ints, pairs)(neg1 ∘ inc1, neg2 ∘ inc2)       # true
# a wrong implementation is caught: swapping the pair *and* adding to the first component
bad2(p) = (p[2] + 1, p[1])
R_fun(R_int, R_int, ints, pairs)(inc1, bad2)                     # false
# Parametricity (Reynolds): any f : ∀a. [a] → [a] commutes with map g, because it is related to itself
# at the relation "graph of g" — a free theorem. For reverse:
g(x) = x^2; xs = [1, 2, 3, 4]
reverse(map(g, xs)) == map(g, reverse(xs))                       # true
```
tab: Lean
```lean
import Mathlib
-- The logical relation at function types, and the composition case of the fundamental lemma.
def RFun {A B C D : Type*} (RA : A → B → Prop) (RC : C → D → Prop) (f : A → C) (g : B → D) : Prop :=
  ∀ a b, RA a b → RC (f a) (g b)

theorem RFun.comp {A B C D E F : Type*} {RA : A → B → Prop} {RC : C → D → Prop} {RE : E → F → Prop}
    {f : A → C} {g : B → D} {f' : C → E} {g' : D → F}
    (h : RFun RA RC f g) (h' : RFun RC RE f' g') : RFun RA RE (f' ∘ f) (g' ∘ g) :=
  fun a b hab => h' _ _ (h a b hab)

-- the difference-pair representation of integers is related to Int, and increment respects it
def Rint (n : ℤ) (p : ℕ × ℕ) : Prop := n = (p.1 : ℤ) - p.2
example : RFun Rint Rint (· + 1) (fun p => (p.1 + 1, p.2)) := by
  intro n p h; simp only [Rint] at *; push_cast; omega

-- a free theorem: reverse commutes with map (Mathlib's List.map_reverse)
example {α β : Type*} (g : α → β) (l : List α) : l.reverse.map g = (l.map g).reverse :=
  List.map_reverse
```
tab: Haskell
```haskell
-- Free theorem for any f :: [a] -> [a]: f . map g == map g . f. Checked for a few such f.
rot :: [a] -> [a]
rot [] = []
rot (x : xs) = xs ++ [x]

everyOther :: [a] -> [a]
everyOther (x : _ : xs) = x : everyOther xs
everyOther xs = xs

main :: IO ()
main = do
  let g = (* 3) :: Int -> Int
      xs = [1 .. 7]
  print [ f (map g xs) == map g (f xs) | f <- [reverse, rot, everyOther, take 3] ]   -- [True,True,True,True]
```
````
