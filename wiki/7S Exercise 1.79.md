#exercise #solution #proof

**Exercise 1.79 (Pullback map).** For monotone $f : P \to Q$, the map $f^* : \mathcal{U}(Q) \to \mathcal{U}(P)$, $U \mapsto f^{-1}(U)$, is monotone. Viewing upper sets as monotone maps to $\mathbb{B}$ ([[Upper Sets Classified by Maps to Bool]]), show $f^*$ is $u \mapsto f \mathbin{;} u$.

## Solution

Let $u : Q \to \mathbb{B}$ classify $U$, i.e. $u(q) = \mathsf{true}$ iff $q \in U$. Then $(f \mathbin{;} u)(p) = \mathsf{true}$ iff $f(p) \in U$ iff $p \in f^{-1}(U)$, so $f \mathbin{;} u$ classifies $f^{-1}(U) = f^*(U)$. This is the preorder version of [[Direct Image, Preimage, and Dual Image|preimage as precomposition]] (Kittenlab Lecture 14).
