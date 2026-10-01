#definition #theorem #example #program

Effectful programs do not form a [[Monoidal Category]], because running $f$ on the left and $g$ on the right of a pair is not one operation: **the order of the effects matters**. A **premonoidal category** (Power & Robinson) keeps the tensor of objects but only lets one act on one side at a time. A **binoidal category** (Román, Definition 2.1) has objects $A \otimes B$ and, separately, functors $A \otimes (-)$ and $(-) \otimes B$ that agree on objects — but $(-) \otimes (-)$ is not required to be a bifunctor. A premonoidal category adds associators and unitors that are natural in each variable separately and **central** (Definition 2.2). A morphism $f$ is *central* if it interchanges with everything:

$$
(f \otimes B') \circ (A \otimes g) \;=\; (A' \otimes g) \circ (f \otimes B) \quad \text{for all } g.
$$

A **Freyd category** is the structure a call-by-value language needs: an identity-on-objects functor $J : \mathcal V \to \mathcal C$ from a cartesian (or monoidal) category of **values / pure maps** into a premonoidal category of **computations**, preserving the premonoidal structure, with central image (Definition 2.4; Román calls the general monoidal case an **effectful category**). Pure maps can be copied, discarded and reordered freely; effectful ones only as the premonoidal structure allows.

> Sources: Power & Robinson, *Premonoidal categories and notions of computation*, Math. Struct. Comp. Sci. 7 (1997); Levy, Power & Thielecke, *Modelling environments in call-by-value programming languages*, Inform. and Comput. 185 (2003) (Freyd categories and their relation to strong monads); Román, *Promonads and String Diagrams for Effectful Categories*, ACT 2022 (EPTCS 380, 2023), [arXiv:2205.07664](https://arxiv.org/abs/2205.07664) ([[Promonads and String Diagrams for Effectful Categories|notes]]) Definitions 2.1, 2.2, 2.4, 2.5, 2.8, 3.7, Theorems 2.14, 3.9; Moggi, *Notions of computation and monads*, Inform. and Comput. 93 (1991).

## From a strong monad

The Kleisli category $\mathcal C_T$ of a strong [[Monad]] $T$ on a cartesian category $\mathcal C$ is premonoidal: $f \otimes B$ uses the strength $A \times TB' \to T(A \times B')$, and $\mathcal C \to \mathcal C_T$ is a Freyd category. Running $f$ then $g$ and running $g$ then $f$ give different composites unless $T$ is *commutative*, which is exactly the condition for $\mathcal C_T$ to be monoidal. The reader and maybe monads are commutative (order of reading or failing does not matter); state, writer and IO are not. Conversely, a Freyd category in which $J$ has a suitable right adjoint comes from a strong monad (Levy–Power–Thielecke), so Freyd categories generalise Moggi's monads to settings that need not have a monad, such as continuations or linear effects.

## String diagrams with a runtime wire

Premonoidal categories still have string diagrams, with one extra wire. Román's **runtime** construction (Definition 2.8) adjoins an object $R$ that every effectful morphism must take as input and produce as output, and shows that an effectful category is a monoidal category with a runtime resource (Theorem 2.14): effects are ordered because they all thread the same wire. Equivalently, effectful categories over $\mathcal V$ are **promonads** over $\mathcal V$ (Definition 3.7, Theorem 3.9) — identity-on-objects functors out of $\mathcal V$ — which puts them in the same framework as [[Profunctor|profunctors]] and [[Profunctor Optics|optics]].

This is the categorical account of a familiar compiler device. Sea-of-nodes and SSA graph IRs thread a **memory / effect token** through every side-effecting operation; pure operations float freely and are reordered or deduplicated by the optimiser. The token is the runtime wire, and "pure operations may float" is centrality.

## Effect systems

Effect rows refine the picture: a morphism annotated with effect $\varepsilon$ commutes with one annotated $\varepsilon'$ when the two effects do not interfere (reading different regions, say). The annotated category is then **graded** by the effects ([[Graded Monad]]), and centrality becomes a property relative to a pair of grades — the formal content of "these two calls can be reordered because their effect rows are disjoint".

## Sophia

Sophia's `EQUIV` edges carry a modulo tag `ordering` for "order of observable effects that are claimed independent": an equivalence that holds only in the monoidal quotient of a premonoidal category. Its effect rows ($\mathit{read}(r)$, $\mathit{write}(r)$, $\mathit{alloc}(r)$, $\mathit{io}$, …) determine which morphisms are central with respect to which, and the design notes ask that the functoriality of frontends be stated for the *effectful* category, i.e. as effectful functors (Definition 2.5). See [Effects Memory and Resources](https://mathstruct.org/Sophia/vault/Design/Effects-Memory-and-Resources) and [Equivalence and Witnesses](https://mathstruct.org/Sophia/vault/Design/Equivalence-and-Witnesses).

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
# Effectful maps A → B as Kleisli arrows of the writer monad: a ↦ (b, log). Composition concatenates logs.
seq(f, g) = a -> ((b, l1) = f(a); (c, l2) = g(b); (c, [l1; l2]))
pure(h) = a -> (h(a), String[])                        # the inclusion of pure functions (identity on objects)
# Premonoidal structure: we can act on ONE side of a pair at a time (binoidal), but not on both at once.
left(f)  = ((a, c),) -> ((b, l) = f(a); ((b, c), l))   # f ⋉ C
right(g) = ((a, c),) -> ((d, l) = g(c); ((a, d), l))   # A ⋊ g
f = a -> (a + 1, ["f"]);  g = c -> (2c, ["g"])
r1 = seq(left(f), right(g))((1, 10))                    # ((2, 20), ["f", "g"])
r2 = seq(right(g), left(f))((1, 10))                    # ((2, 20), ["g", "f"])
r1 == r2                                                # false: no interchange law, so ⊗ is not a bifunctor
r1[1] == r2[1]                                          # true: the values agree, the order of effects does not
# Pure maps are *central*: they interchange with everything — the Freyd category's pure part is monoidal.
p = pure(a -> a * 3)
seq(left(p), right(g))((1, 10)) == seq(right(g), left(p))((1, 10))   # true
```
tab: Lean
```lean
import Mathlib
-- In the Kleisli category of the state monad, the two orders of two effects differ:
def incThenDouble : StateM Nat Unit := do modify (· + 1); modify (· * 2)
def doubleThenInc : StateM Nat Unit := do modify (· * 2); modify (· + 1)
example : (incThenDouble.run 1).2 = 4 := rfl
example : (doubleThenInc.run 1).2 = 3 := rfl
-- Pure functions (lifted with `pure`) are central: they commute with any effect.
example (f : Nat → Nat) (m : StateM Nat Nat) :
    (do let a ← m; let b ← pure (f 0); pure (a, b)) =
    (do let b ← pure (f 0); let a ← m; pure (a, b)) := by
  simp
```
tab: Haskell
```haskell
-- Kleisli arrows of the writer monad: the interchange law fails for effectful maps.
type Eff a b = a -> (b, [String])

seqE :: Eff a b -> Eff b c -> Eff a c
seqE f g a = let (b, l1) = f a; (c, l2) = g b in (c, l1 ++ l2)

leftE :: Eff a b -> Eff (a, c) (b, c)
leftE f (a, c) = let (b, l) = f a in ((b, c), l)

rightE :: Eff c d -> Eff (a, c) (a, d)
rightE g (a, c) = let (d, l) = g c in ((a, d), l)

main :: IO ()
main = do
  let f a = (a + 1, ["f"]) :: (Int, [String])
      g c = (2 * c, ["g"]) :: (Int, [String])
  print (seqE (leftE f) (rightE g) (1, 10))   -- ((2,20),["f","g"])
  print (seqE (rightE g) (leftE f) (1, 10))   -- ((2,20),["g","f"])
```
````
