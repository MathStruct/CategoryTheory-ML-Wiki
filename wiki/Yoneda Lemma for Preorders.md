#theorem #proof #exercise

For a [[Preorder]] $(P, \leq)$ and $p \in P$, the **principal upper set** is $\uparrow p := \{p' \in P \mid p \leq p'\}$. Then:

1. $\uparrow p$ is an [[Upper Set]];
2. $\uparrow : P^{\mathrm{op}} \to \mathcal{U}(P)$ is a [[Monotone Map]] (from the [[Opposite Preorder]]);
3. $p \leq p'$ in $P$ if and only if $\uparrow p' \subseteq \uparrow p$.

> Source: 7 Sketches Exercise 1.66 ("the Yoneda lemma for preorders"); Kittenlab Lecture 10, 12.

*Proof.* (1) If $q \in \uparrow p$ and $q \leq q'$ then $p \leq q \leq q'$, so $q' \in \uparrow p$. (2) If $p \leq q$ then for $q' \in \uparrow q$ we get $p \leq q \leq q'$, so $\uparrow q \subseteq \uparrow p$; this is monotonicity from $P^{\mathrm{op}}$. (3) Monotonicity gives one direction; conversely $p' \in \uparrow p'$ always, so $\uparrow p' \subseteq \uparrow p$ forces $p' \in \uparrow p$, i.e. $p \leq p'$. $\blacksquare$

"Up to equivalence, to know an element is the same as knowing its upper set — its web of relationships with the other elements." The general [[Yoneda Lemma]] says the same for a [[Category]]: an object is determined by its [[Representable Functor|representable functor]] $\mathrm{Hom}(x, -)$ (Kittenlab: $y_{\mathcal{C}}(i)(j) = 1$ if $i \leq j$ and $\varnothing$ otherwise, so $y(i) \cong y(j)$ iff $i \cong j$). Upper sets $\mathcal{U}(P)$ are the $\mathbb{B}$-valued [[C-Set|copresheaves]], and $\uparrow$ is the [[Yoneda Embedding]].

Picture for $P = (b \geq a \leq c)$: $\uparrow a = \{a, b, c\}$, $\uparrow b = \{b\}$, $\uparrow c = \{c\}$; the map $\uparrow$ reverses the order.

````tabs
tab: Julia
```julia
principal_up(leq, xs, p) = Set(q for q in xs if leq(p, q))
# Yoneda: p ≤ p' iff ↑p' ⊆ ↑p
xs = [:a, :b, :c]; leq(x, y) = x == y || x == :a
all(leq(p, q) == issubset(principal_up(leq, xs, q), principal_up(leq, xs, p)) for p in xs, q in xs)
```
tab: Lean
```lean
#check @Set.Ici              -- ↑p as a set
#check @Set.Ici_subset_Ici   -- Ici a ⊆ Ici b ↔ b ≤ a   (part 3)
#check @UpperSet.Ici          -- as a bundled upper set
```
tab: Haskell
```haskell
principalUp :: Preorder a => [a] -> a -> [a]
principalUp xs p = [q | q <- xs, leq p q]

yonedaPre :: (Preorder a, Eq a) => [a] -> a -> a -> Bool
yonedaPre xs p p' = all (`elem` principalUp xs p) (principalUp xs p')   -- == leq p p'
```
````
