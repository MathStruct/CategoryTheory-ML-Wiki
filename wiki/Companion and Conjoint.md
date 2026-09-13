#definition #example #theorem

Let $F : \mathcal{P} \to \mathcal{Q}$ be a $\mathcal{V}$-[[Enriched Functor|functor]]. Its **companion** $\hat F : \mathcal{P} \nrightarrow \mathcal{Q}$ and **conjoint** $\check F : \mathcal{Q} \nrightarrow \mathcal{P}$ are the [[Profunctor|profunctors]]

$$
\hat F(p, q) := \mathcal{Q}(F(p), q), \qquad \check F(q, p) := \mathcal{Q}(q, F(p)).
$$

In the $\mathbf{Bool}$ case a [[Monotone Map]] is "a bunch of arrows, one from each $p$ landing at $F(p)$" — which looks exactly like bridges: that is the companion; mentally reversing every dotted arrow gives bridges from $\mathcal{Q}$ to $\mathcal{P}$, the conjoint. This is how [[Profunctor|profunctors]] generalize functors.

> Sources: 7 Sketches §4.3.3 (Definition 4.34, Examples 4.35, 4.37, Remark 4.39, Exercises 4.36, 4.38, 4.41); [Shu08].

- The companion and conjoint of $\mathrm{id} : \mathcal{P} \to \mathcal{P}$ both equal the unit profunctor $U_{\mathcal{P}}$ (Example 4.35, [[7S Chapter 4 Exercises#Exercise 4.36|7S Exercise 4.36]]).
- $+ : \mathbb{R}^3 \to \mathbb{R}$ is monotone; its companion sends $(a, b, c, d)$ to $[a + b + c \leq d]$ (Example 4.37) and its conjoint $\check{+}(d, (a, b, c)) = [d \leq a + b + c]$ ([[7S Chapter 4 Exercises#Exercise 4.38|7S Exercise 4.38]]). These are the $\Sigma$ boxes in [[Co-design]] diagrams — "not to be designed, but they fit easily into the same framework".
- **$\mathcal{V}$-adjunctions** (Remark 4.39): $\mathcal{V}$-functors $F : \mathcal{P} \to \mathcal{Q}$, $G : \mathcal{Q} \to \mathcal{P}$ are adjoint if $\mathcal{P}(p, G q) \cong \mathcal{Q}(F p, q)$ for all $p, q$ (Eq. 4.40) — generalizing [[Galois Connection|Galois connections]] from $\mathbf{Bool}$ to any $\mathcal{V}$. **Theorem** ([[7S Chapter 4 Exercises#Exercise 4.41|7S Exercise 4.41]]): $F \dashv G$ iff $\hat F = \check G$, i.e. $\mathcal{Q}(F p, q) = \mathcal{P}(p, G q)$; applied to $\mathrm{id} \dashv \mathrm{id}$ this gives $\widehat{\mathrm{id}} = \check{\mathrm{id}}$.
- In $\mathbf{Set}$-profunctor language, $\hat F = \mathcal{Q}(F-, -)$ and $\check F = \mathcal{Q}(-, F-)$ are the *representable* profunctors; $F \dashv G$ iff $\mathcal{Q}(F-, -) \cong \mathcal{P}(-, G-)$ — the hom-set definition of [[Adjunction]]. Companions/conjoints make $\mathbf{Prof}$ a *proarrow equipment* (7 Sketches Preface, §4.6).

````tabs
tab: Julia
```julia
# companion and conjoint of a monotone map F between finite preorders (as Bool matrices)
companion(F, P, Q, leqQ) = Bool[leqQ(F(p), q) for p in P, q in Q]      # F̂(p, q) = [F p ≤ q]
conjoint(F, P, Q, leqQ)  = Bool[leqQ(q, F(p)) for q in Q, p in P]      # F̌(q, p) = [q ≤ F p]
# Galois connection test (Exercise 4.41): F ⊣ G iff companion(F) == transpose(conjoint(G))... i.e. [F p ≤ q] == [p ≤ G q]
```
tab: Haskell
```haskell
companion :: Preorder q => (p -> q) -> Feas p q
companion f = Feas (\p q -> leq (f p) q)
conjoint :: Preorder q => (p -> q) -> Feas q p
conjoint f = Feas (\q p -> leq q (f p))
```
````
