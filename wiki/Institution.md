#definition #theorem #example #program

An **institution** (Goguen & Burstall) is an abstract notion of "a logic", general enough to talk about translations *between* logics. It consists of

- a category $\mathbf{Sign}$ of **signatures** and signature morphisms (vocabularies and renamings);
- a functor $\mathrm{Sen} : \mathbf{Sign} \to \mathbf{Set}$ giving the **sentences** over each signature — sentences translate *forward* along a renaming;
- a functor $\mathrm{Mod} : \mathbf{Sign}^{\mathrm{op}} \to \mathbf{Cat}$ giving the **models** — models translate *backward*, by forgetting (the **reduct**);
- for each $\Sigma$ a **satisfaction** relation $\models_\Sigma \;\subseteq |\mathrm{Mod}(\Sigma)| \times \mathrm{Sen}(\Sigma)$,

subject to the **satisfaction condition**: for every $\sigma : \Sigma \to \Sigma'$, every $\Sigma'$-model $M'$ and every $\Sigma$-sentence $\varphi$,

$$
M' \models_{\Sigma'} \mathrm{Sen}(\sigma)(\varphi) \quad\Longleftrightarrow\quad \mathrm{Mod}(\sigma)(M') \models_{\Sigma} \varphi.
$$

**Truth is invariant under change of notation.** First-order logic, equational logic, Horn clauses, propositional logic, modal logics, type theories and Hoare logics are all institutions.

> Sources: Goguen & Burstall, *Institutions: abstract model theory for specification and programming*, J. ACM 39(1) (1992); Goguen & Roşu, *Institution morphisms*, Formal Aspects of Computing 13 (2002); Tarlecki, *Moving between logical systems*, WADT 1995 (LNCS 1130, 1996) (comorphisms and heterogeneous specification); Mossakowski, Maeder & Lüttich, *The Heterogeneous Tool Set (Hets)*, TACAS 2007; Diaconescu, *Institution-independent Model Theory* (Birkhäuser 2008). Categorical background: [[Functor]], [[Contravariant Functor]], [[Category of Categories]], and [[Functorial Semantics]].

## The shape: a contravariance and a covariance

Sentences go forward and models go backward along the same renaming, and satisfaction is a "pairing" between them that the renaming preserves — the same pattern as a [[Galois Connection]] or the duality between syntax and semantics in [[Functorial Semantics]]. Categorically, an institution is a functor $\mathbf{Sign} \to \mathbf{Room}$ into the category of "rooms" (a set of sentences, a category of models, a satisfaction relation) whose morphisms are "corridors" satisfying the condition — an instance of a [[Grothendieck Construction|fibred]] picture in which a logic is indexed by its vocabularies.

## Morphisms and comorphisms

There are two ways to relate institutions $I$ and $J$:

| | signatures | sentences | models | use |
|---|---|---|---|---|
| **institution morphism** $I \to J$ | $\Phi : \mathbf{Sign}_I \to \mathbf{Sign}_J$ | $J \to I$ | $I \to J$ | $I$ is built on $J$: forget structure |
| **institution comorphism** $I \to J$ | $\Phi : \mathbf{Sign}_I \to \mathbf{Sign}_J$ | $I \to J$ | $J \to I$ | **encode** $I$ in $J$: translate specifications |

each with its own satisfaction condition, e.g. for a comorphism $M' \models_{\Phi\Sigma} \alpha(\varphi) \iff \beta(M') \models_\Sigma \varphi$. Comorphisms are what a tool needs to *reuse a prover*: translate an $I$-specification into $J$, prove it there, and the satisfaction condition guarantees the result means what it should in $I$. Hets implements dozens of logics and comorphisms between them, and proves heterogeneous specifications by routing each part to a suitable prover.

## Why it matters for compilers

A compiler frontend is a translation of a source language into a core language, and the obligation it must discharge — "a property proved of the core term holds of the source program, and conversely" — is the satisfaction condition of a **comorphism**. Ordinary compiler-correctness statements ([[Compiler Correctness]]) are about *programs*; the institution view adds *specifications*: what is preserved is the meaning of every sentence one can state about the program.

## Sophia

Sophia's frontends translate Julia, C++ and Lean into one Core Calculus and store proofs about core terms, so each frontend should be an institution comorphism from its source "logic" (programs plus their specifications) into Sophia's: the satisfaction condition is exactly the requirement that a stored witness about a core term is a fact about the source program. See [Multi-AST Layering](https://mathstruct.org/Sophia/vault/Design/Multi-AST-Layering) and [Equivalence and Witnesses](https://mathstruct.org/Sophia/vault/Design/Equivalence-and-Witnesses).

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# The institution of propositional logic.
#   Sign: sets of atoms;  Sen(Σ): formulas over Σ;  Mod(Σ): valuations Σ → Bool;  ⊨: evaluation.
#   A signature morphism σ : Σ → Σ′ translates sentences forward (Sen σ) and models backward (Mod σ = reduct).
sat(v, φ) = φ isa Symbol ? v[φ] :
            φ[1] == :not ? !sat(v, φ[2]) :
            φ[1] == :and ? sat(v, φ[2]) && sat(v, φ[3]) : (sat(v, φ[2]) || sat(v, φ[3]))
sen(σ, φ) = φ isa Symbol ? σ[φ] : (φ[1], (sen(σ, x) for x in φ[2:end])...)    # rename atoms
mod_(σ, v′) = Dict(p => v′[σ[p]] for p in keys(σ))                            # reduct: precompose with σ
Σ = [:p, :q]; Σ′ = [:a, :b, :c]
σ = Dict(:p => :a, :q => :a)                     # not injective: p and q are both sent to a
φ = (:or, (:and, :p, (:not, :q)), :q)            # (p ∧ ¬q) ∨ q
sen(σ, φ)                                         # (a ∧ ¬a) ∨ a
valuations(S) = [Dict(zip(S, bits)) for bits in Iterators.product(ntuple(_ -> (false, true), length(S))...)]
# The satisfaction condition: M′ ⊨ Sen(σ)(φ)  ⟺  Mod(σ)(M′) ⊨ φ, for every model M′ of Σ′
all(sat(v′, sen(σ, φ)) == sat(mod_(σ, v′), φ) for v′ in valuations(Σ′))      # true
length(valuations(Σ′))                                                     # 8 models checked
```
tab: Lean
```lean
import Mathlib
-- Sentences of propositional logic over a signature (a type of atoms)
inductive Fml (S : Type) where
  | atom : S → Fml S
  | neg : Fml S → Fml S
  | conj : Fml S → Fml S → Fml S

-- Sen(σ): translate sentences along a signature morphism
def Fml.map {S₁ S₂ : Type} (σ : S₁ → S₂) : Fml S₁ → Fml S₂
  | .atom p => .atom (σ p)
  | .neg φ => .neg (φ.map σ)
  | .conj φ ψ => .conj (φ.map σ) (ψ.map σ)

-- satisfaction: a model is a valuation
def Fml.sat {S : Type} (v : S → Prop) : Fml S → Prop
  | .atom p => v p
  | .neg φ => ¬ φ.sat v
  | .conj φ ψ => φ.sat v ∧ ψ.sat v

-- the satisfaction condition: M' ⊨ Sen(σ) φ  ↔  Mod(σ) M' ⊨ φ, where Mod(σ) M' = M' ∘ σ
theorem satisfaction {S₁ S₂ : Type} (σ : S₁ → S₂) (v : S₂ → Prop) (φ : Fml S₁) :
    (φ.map σ).sat v ↔ φ.sat (v ∘ σ) := by
  induction φ with
  | atom p => rfl
  | neg φ ih => simp only [Fml.map, Fml.sat, ih]
  | conj φ ψ ih₁ ih₂ => simp only [Fml.map, Fml.sat, ih₁, ih₂]
```
````
