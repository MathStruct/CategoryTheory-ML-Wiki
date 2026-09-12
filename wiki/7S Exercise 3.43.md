#exercise #solution #proof

**Exercise 3.43.** Construct the category $\mathbf{Cat}$: identity functors, composition of functors, and the category axioms.

## Solution

1. $\mathrm{id}_{\mathcal{C}}$ fixing every object and morphism preserves identities and composites trivially.
2. For $F : \mathcal{C} \to \mathcal{D}$, $G : \mathcal{D} \to \mathcal{E}$, $(F \mathbin{;} G)(\mathrm{id}_c) = G(F(\mathrm{id}_c)) = G(\mathrm{id}_{Fc}) = \mathrm{id}_{GFc}$ and $(F \mathbin{;} G)(f \mathbin{;} g) = G(Ff \mathbin{;} Fg) = GFf \mathbin{;} GFg$.
3. Unitality and associativity hold because they hold for the underlying functions on objects and hom-sets: $((F \mathbin{;} G) \mathbin{;} H)(x) = H(G(F(x))) = (F \mathbin{;} (G \mathbin{;} H))(x)$. See [[Category of Categories]].
