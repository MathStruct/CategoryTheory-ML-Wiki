#definition #example

Let $(P, \leq_P)$ and $(Q, \leq_Q)$ be [[Preorder|preorders]]. A [[Monotone Map|monotone]] function $f : P \to Q$ is an **isomorphism** if there is a monotone $g : Q \to P$ with $f \mathbin{;} g = \mathrm{id}_P$ and $g \mathbin{;} f = \mathrm{id}_Q$, i.e. $p = g(f(p))$ and $q = f(g(q))$. $g$ is the **inverse** of $f$. If an isomorphism exists, $P$ and $Q$ are **isomorphic**. An isomorphism of preorders is "basically just a relabeling of the elements".

> Sources: 7 Sketches Definition 1.75, Example 1.76, Remark 1.74; this is the special case of [[Isomorphism]] in the [[Category of Preorders]].

**Example 1.76.** The preorders $P$ ($a \leq b, c \leq d \leq e$ with $b, c$ incomparable), $Q$ ($v, w \leq x \leq y \leq z$-shaped) and $R$ (same as $Q$ but with an extra drawn arrow $x \to z$) are all isomorphic; the extra arrow is implied by transitivity, so $Q$ and $R$ are literally the same preorder.

A [[Galois Connection]] is a "relaxed isomorphism": replacing $\leq$ by $=$ in $p \leq g(f(p))$, $f(g(q)) \leq q$ recovers this definition. Compare [[Equivalence of Categories]], of which isomorphism of *skeletal* preorders is a special case: two preorders are equivalent as categories iff their [[Partial Order|poset reflections]] are isomorphic.

````tabs
tab: Lean
```lean
#check (OrderIso ℕ ℕ)     -- α ≃o β : an order-preserving bijection with order-preserving inverse
#check @OrderIso.symm
```
tab: Haskell
```haskell
data OrderIso a b = OrderIso (Monotone a b) (Monotone b a)
-- laws: the two composites are identities
```
````
