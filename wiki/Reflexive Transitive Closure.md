#definition #example #theorem

**Level shifting** (7 Sketches §1.4.5): for any [[Set]] $S$ there is a [[Preorder]] $\mathrm{Rel}(S)$ of all binary [[Relation|relations]] on $S$, ordered by inclusion $R \subseteq R'$. There is also a set $\mathrm{Pos}(S)$ of all *preorder relations* on $S$, itself a preorder under inclusion $\leq \sqsubseteq \leq'$ (iff $a \leq b$ implies $a \leq' b$) — "a preorder of preorder structures: that's what we mean by a level shift".

> Sources: 7 Sketches §1.4.5, Exercises 1.124, 1.125.

Every preorder relation is a relation, giving an inclusion $U : \mathrm{Pos}(S) \to \mathrm{Rel}(S)$. This is the right adjoint of a [[Galois Connection]] whose left adjoint
$$\mathrm{Cl} : \mathrm{Rel}(S) \to \mathrm{Pos}(S)$$
takes a relation $R$ to its **reflexive and transitive closure**: add $s \leq s$ for every $s$ and $s \leq u$ whenever $s \leq t$ and $t \leq u$. The adjunction says $\mathrm{Cl}(Q) \sqsubseteq {\leq}$ iff $Q \subseteq U(\leq)$: a preorder contains the closure of $Q$ exactly when it contains $Q$ ([[7S Exercise 1.125]]). The composite $U \circ \mathrm{Cl}$ is a [[Closure Operator]] on $\mathrm{Rel}(S)$.

**Example.** $\mathrm{Rel}(\{1\})$ has two elements $\varnothing \leq \{(1,1)\}$; $\mathrm{Rel}(\{1,2\})$ has 16 ([[7S Exercise 1.124]]).

This is the preorder analogue of the [[Free Category]] on a [[Graph]] (the reflexive-transitive closure *with named paths*), of the [[Free Monoid]], and of the [[Hasse Diagram]] construction: a graph presents the preorder $\mathrm{Cl}(A)$.

````tabs
tab: Julia
```julia
# reflexive transitive closure of a relation on {1..n} given as a BitMatrix (Warshall)
function refl_trans_closure(R::BitMatrix)
  n = size(R, 1); C = copy(R)
  for i in 1:n; C[i,i] = true; end
  for k in 1:n, i in 1:n, j in 1:n
    C[i,j] |= C[i,k] && C[k,j]
  end
  C
end
R = BitMatrix([0 1 0; 0 0 1; 0 0 0])
refl_trans_closure(R)      # the chain 1 ≤ 2 ≤ 3
```
tab: Lean
```lean
#check @Relation.ReflTransGen      -- inductive reflexive-transitive closure
#check @Relation.ReflTransGen.mono
-- the adjunction: ReflTransGen r ≤ s ↔ r ≤ s for a preorder s (transitive & reflexive)
#check @Relation.reflTransGen_eq_self
```
tab: Haskell
```haskell
-- closure of a finite relation (as a list of pairs) on a finite carrier
reflTransClosure :: Eq a => [a] -> [(a, a)] -> [(a, a)]
reflTransClosure xs r = fixpoint step (r ++ [(x, x) | x <- xs])
  where
    step s = foldr addNew s [ (a, c) | (a, b) <- s, (b', c) <- s, b == b' ]
    addNew p s = if p `elem` s then s else p : s
    fixpoint f s = let s' = f s in if length s' == length s then s else fixpoint f s'
```
````
