#solution #proof

**Solution to [[7S Exercise 3.43|Exercise 3.43]].**

1. $\mathrm{id}_{\mathcal{C}}$ fixing every object and morphism preserves identities and composites trivially.
2. For $F : \mathcal{C} \to \mathcal{D}$, $G : \mathcal{D} \to \mathcal{E}$, $(F \mathbin{;} G)(\mathrm{id}_c) = G(F(\mathrm{id}_c)) = G(\mathrm{id}_{Fc}) = \mathrm{id}_{GFc}$ and $(F \mathbin{;} G)(f \mathbin{;} g) = G(Ff \mathbin{;} Fg) = GFf \mathbin{;} GFg$.
3. Unitality and associativity hold because they hold for the underlying functions on objects and hom-sets: $((F \mathbin{;} G) \mathbin{;} H)(x) = H(G(F(x))) = (F \mathbin{;} (G \mathbin{;} H))(x)$. See [[Category of Categories]].

> Sources: 7 Sketches, Exercise 3.43 and Solution A.3.
