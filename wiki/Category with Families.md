#definition #theorem #example #program

A **category with families** (cwf, Dybjer) is the algebraic structure of a dependent type theory with its contexts and substitutions. It consists of (Castellan, Clairambault & Dybjer, Definition 1):

- a category $\mathcal C$ of **contexts** $\Gamma, \Delta$ and **substitutions** $\sigma : \Delta \to \Gamma$, with a terminal object (the empty context);
- a functor $T : \mathcal C^{\mathrm{op}} \to \mathbf{Fam}$ assigning to each context a set $\mathrm{Ty}(\Gamma)$ of **types** and, for each $A \in \mathrm{Ty}(\Gamma)$, a set $\mathrm{Tm}(\Gamma, A)$ of **terms**; functoriality is **substitution** $A[\sigma]$, $t[\sigma]$;
- **context comprehension**: for each $A \in \mathrm{Ty}(\Gamma)$ an extended context $\Gamma . A$, a projection $p : \Gamma . A \to \Gamma$ and a generic term $q \in \mathrm{Tm}(\Gamma . A, A[p])$, universal in the sense that substitutions $\Delta \to \Gamma . A$ correspond bijectively to pairs $(\sigma : \Delta \to \Gamma,\; t \in \mathrm{Tm}(\Delta, A[\sigma]))$, written $\langle \sigma, t \rangle$.

Context extension is how dependency enters: a type in context $\Gamma . A$ may mention a variable of type $A$. Type formers — $\Pi$, $\Sigma$, identity types, universes — are extra structure on a cwf, stable under substitution. The **term model** (syntax modulo judgemental equality) is the initial cwf with that structure (Theorems 6–7, 10), so a cwf with $\Pi$-types is precisely a model of dependent type theory with $\Pi$-types, and the interpretation of syntax is the unique structure-preserving map out of the term model.

> Sources: Dybjer, *Internal type theory*, TYPES 1995 (LNCS 1158, 1996); Castellan, Clairambault & Dybjer, *Categories with Families: Unityped, Simply Typed, and Dependently Typed*, [arXiv:1904.00827](https://arxiv.org/abs/1904.00827) ([[Categories with Families - Unityped, Simply Typed, and Dependently Typed|notes]]) Definitions 1–4, 9, Theorems 1–12, Proposition 1; Awodey, *Natural models of homotopy type theory*, Math. Struct. Comp. Sci. (2016), [arXiv:1406.3219](https://arxiv.org/abs/1406.3219) ([[Natural Models of Homotopy Type Theory|notes]]) Definition 1, Proposition 2, Theorem 16. DaoFP Ch. 11 and [[Dependent Type]], [[Locally Cartesian Closed Category]] for the categorical background.

## Natural models: a cwf is one map of presheaves

Awodey's reformulation packages a cwf as a single [[Natural Transformation]] of [[Presheaf|presheaves]] on $\mathcal C$,

$$
p : \dot{\mathcal U} \to \mathcal U, \qquad \mathcal U(\Gamma) = \mathrm{Ty}(\Gamma), \quad \dot{\mathcal U}(\Gamma) = \textstyle\coprod_{A} \mathrm{Tm}(\Gamma, A),
$$

that is **representable** (Definition 1): every fibre over a type $A : y\Gamma \to \mathcal U$ is representable, by $y(\Gamma . A)$ — the pullback of $p$ along $A$ is the comprehension. Proposition 2: $p$ is representable iff $(\mathcal C, p)$ is a cwf. Type formers become operations on this one map — $\Pi$ is a pullback square involving the polynomial functor of $p$ ([[Polynomial Functor]]) — and Theorem 16 gives the natural-model version of extensional Martin-Löf type theory. The view "a type theory is a representable map of presheaves" is the one modern proof assistants' semantics are written in.

## The same picture at three levels

Castellan et al. show the cwf notion specialises cleanly:

| cwf | equivalent to | Theorem |
|---|---|---|
| unityped, contextual | cartesian operads, [[Lawvere Theory|Lawvere theories]] | 1–2 |
| simply typed, contextual | cartesian categories; with $\lambda\beta\eta$, CCCs ([[Curry-Howard-Lambek Correspondence]]) | 3–5 |
| dependently typed, democratic, with $\Sigma$ and extensional identity types | finitely complete categories; adding $\Pi$: LCCCs (biequivalences) | 8–9 |

and the base category of the initial cwf with extensional identity types, $\Sigma$ and $\Pi$ is the **free locally cartesian closed category** on one object (Theorem 11), in which equality is **undecidable** (Theorem 12). This is the precise reason dependent type checkers use *intensional* identity types: extensional equality cannot be decided, so it cannot be part of a decidable typing judgement.

## Definitional versus propositional equality

The equations of a cwf ($A[\sigma][\tau] = A[\sigma\tau]$, $p \circ \langle \sigma, t\rangle = \sigma$, $\beta$, $\eta$) hold *strictly*: they are **definitional** equality, decided by the type checker by normalisation and invisible in proofs. **Propositional** equality is a type, $\mathrm{Id}_A(s, t)$, whose terms are proofs that must be constructed. A cwf model interprets the first as equality of morphisms and the second as an object.

## Sophia

Sophia's Core Calculus is a dependent type theory, so its semantics is a cwf, and its central design split follows Theorem 12: definitional equality is decided by normalisation and **folded into the hash** (definitionally equal terms get one node), while propositional equalities are stored as `EQUIV` edges with witnesses, because they cannot be decided. See [Core Calculus](https://mathstruct.org/Sophia/vault/Design/Core-Calculus) and [Hashing and Identity](https://mathstruct.org/Sophia/vault/Design/Hashing-and-Identity).

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# The set model of a category with families: contexts are finite sets, a type in context Γ is a family
# (γ ↦ set A(γ)), a term of type A is a dependent function (a section), substitution is precomposition.
Γ = [1, 2, 3]
A = Dict(1 => [:a], 2 => [:b, :c], 3 => Symbol[])          # a type in context Γ: A(3) is empty
subst(A, σ, Δ) = Dict(δ => A[σ(δ)] for δ in Δ)              # A[σ] for σ : Δ → Γ
Δ = [:x, :y]; σ = Dict(:x => 1, :y => 2); Θ = [0]; τ = Dict(0 => :y)
# functoriality of substitution in types: A[σ][τ] = A[σ ∘ τ]
subst(subst(A, d -> σ[d], Δ), t -> τ[t], Θ) == subst(A, t -> σ[τ[t]], Θ)    # true
# context comprehension Γ.A = Σ_{γ ∈ Γ} A(γ), with projection p : Γ.A → Γ and generic term q of type A[p]
ΓA = [(γ, a) for γ in Γ for a in A[γ]]                       # [(1,:a), (2,:b), (2,:c)]
p(e) = e[1]; q(e) = e[2]
# universal property: a substitution Δ → Γ.A is the same as a pair (σ : Δ → Γ, t ∈ Tm(Δ, A[σ]))
t = Dict(:x => :a, :y => :c)                                 # a term: t(δ) ∈ A(σ(δ))
all(t[δ] in A[σ[δ]] for δ in Δ)                              # true: t is well-typed
ext(δ) = (σ[δ], t[δ])                                        # the extension ⟨σ, t⟩ : Δ → Γ.A
all(ext(δ) in ΓA && p(ext(δ)) == σ[δ] && q(ext(δ)) == t[δ] for δ in Δ)   # true: p∘⟨σ,t⟩ = σ, q[⟨σ,t⟩] = t
```
tab: Lean
```lean
import Mathlib
-- The set model of a cwf in Lean's own type theory: a type in context Γ is a family Γ → Type,
-- a term is a dependent function, comprehension is the Σ-type, p is Sigma.fst, q is Sigma.snd.
section
variable {Γ Δ : Type} (A : Γ → Type)
def ext (σ : Δ → Γ) (t : ∀ δ, A (σ δ)) : Δ → Σ γ, A γ := fun δ => ⟨σ δ, t δ⟩
example (σ : Δ → Γ) (t : ∀ δ, A (σ δ)) : Sigma.fst ∘ ext A σ t = σ := rfl
example (σ : Δ → Γ) (t : ∀ δ, A (σ δ)) (δ : Δ) : (ext A σ t δ).2 = t δ := rfl
-- substitution in types is precomposition, and it is functorial on the nose (definitionally)
example {Θ : Type} (σ : Δ → Γ) (τ : Θ → Δ) : (A ∘ σ) ∘ τ = A ∘ (σ ∘ τ) := rfl
end
```
````
