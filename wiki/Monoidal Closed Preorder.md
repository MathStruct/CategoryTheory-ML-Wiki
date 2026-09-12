#definition #example #theorem #proof

A [[Symmetric Monoidal Preorder]] $\mathcal{V} = (V, \leq, I, \otimes)$ is **symmetric monoidal closed** (or just **closed**) if for every $v, w \in V$ there is an element $v \multimap w \in V$, the **hom-element**, such that
$$(a \otimes v) \leq w \quad\text{iff}\quad a \leq (v \multimap w) \qquad (2.80)$$
for all $a, v, w$. "Closed" means the preorder is closed under "taking homs". Think of $v \multimap w$ as a *single-use $v$-to-$w$ converter*: $a$ and $v$ suffice to get $w$ iff $a$ suffices to get a single-use converter.

> Sources: 7 Sketches §2.5.1, Definition 2.79, Remark 2.81, 2.89, Examples 2.83, 2.85, 2.86, Proposition 2.87, 2.98, Exercises 2.82, 2.84; DaoFP §19.1, §20.1 ("Self-enrichment"); related: [[Compact Closed Category]], [[Cartesian Closed Category]], [[Monoidal Closed Category]].

## Examples

- [[Cost]]: $x \multimap y = \max(0, y - x)$ (Example 2.83) — subtraction defined from order and product.
- [[Bool (Monoidal Preorder)|$\mathbf{Bool}$]]: $v \multimap w = (v \Rightarrow w)$, implication ([[7S Exercise 2.84]]).
- $(\mathcal{P}(S), \subseteq, S, \cap)$: $B \multimap C = \overline{B} \cup C$ ([[7S Exercise 2.94]]).
- Non-example: $(\mathbb{B}, \leq, \mathsf{false}, \vee)$ (Example 2.85).
- Chemistry is not closed, but $2\mathrm{Na} \multimap (2\mathrm{NaOH} + \mathrm{H_2})$ would be a "potential reaction" (Example 2.86, [[Resource Theory]]).

## Closedness is an adjunction ([[7S Exercise 2.82]])

Condition (2.80) says exactly that $(- \otimes v) : V \to V$ is left adjoint to $(v \multimap -) : V \to V$ in a [[Galois Connection]], once both are monotone: $(- \otimes v)$ is monotone by axiom (a); from reflexivity $(v \multimap w) \leq (v \multimap w)$ we get $(v \multimap w) \otimes v \leq w$, and then $u \leq u'$ gives $(v \multimap u) \otimes v \leq u \leq u'$, hence $(v \multimap u) \leq (v \multimap u')$.

## Proposition 2.87

For closed $\mathcal{V}$:
(a) $(- \otimes v) \dashv (v \multimap -)$;
(b) $\otimes$ distributes over joins: $v \otimes \bigvee_{a \in A} a \cong \bigvee_{a \in A} (v \otimes a)$ whenever $\bigvee A$ exists (left adjoints preserve joins, [[Right Adjoints Preserve Meets]]);
(c) $v \otimes (v \multimap w) \leq w$ — "a $v$ and a $v$-to-$w$ converter give a $w$" (counit, plus symmetry);
(d) $v \cong (I \multimap v)$ — "a $v$ is a nothing-to-$v$ converter" (from $v \otimes I \leq v$ and (c));
(e) $(u \multimap v) \otimes (v \multimap w) \leq (u \multimap w)$ — converters compose (apply (c) twice).

**Self-enrichment** (Remark 2.89): $\mathcal{V}(v, w) := v \multimap w$ makes $\mathcal{V}$ a $\mathcal{V}$-[[Enriched Category|category]]: $I \leq (x \multimap x)$ since $I \otimes x \leq x$, and (e) is composition. "Before you can really enrich others, you should really enrich yourself." DaoFP §20.1: any monoidal closed *category* is self-enriched via internal homs $[a, b]$, with composition from the evaluation counit.

**Proposition 2.98.** If $\mathcal{V}$ has all joins, then $\mathcal{V}$ is closed iff $\otimes$ distributes over joins (2.88); then $v \multimap w = \bigvee \{a \mid a \otimes v \leq w\}$ by the [[Adjoint Functor Theorem for Preorders]]. A closed preorder with all joins is a [[Quantale]].

````tabs
tab: Julia
```julia
# closed structure computed from joins on a finite quantale: v ⊸ w = ⋁{a | a ⊗ v ≤ w}
hom_from_joins(V, elems, v, w) = join(V, [a for a in elems if leq(V, otimes(V, a, v), w)])

# check the adjunction (2.80) on samples
is_closed(V, elems) = all(leq(V, otimes(V, a, v), w) == leq(V, a, hom(V, v, w)) for a in elems, v in elems, w in elems)
is_closed(BoolPre(), [false, true])                          # true
is_closed(CostPre(), [0.0, 1.0, 2.5, Inf])                   # true
```
tab: Lean
```lean
-- Mathlib: `HImp` / `HeytingAlgebra` is the cartesian case (⊓ ⊣ ⇨);
-- `OrderedCommMonoid` + residuation: see `Order.Quantale` (Mathlib.Algebra.Order.Quantale)
#check @le_himp_iff              -- a ≤ b ⇨ c ↔ a ⊓ b ≤ c
#check IsQuantale                -- mul distributes over sSup; residuals exist (leftResiduation)
```
tab: Haskell
```haskell
class MonoidalPreorder v => Closed v where
  hom :: v -> v -> v            -- v ⊸ w, with  leq (a <> v) w == leq a (hom v w)

instance Closed All  where hom (All v) (All w) = All (not v || w)
instance Closed Cost where hom = homCost
```
````
