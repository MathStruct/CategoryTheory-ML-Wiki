#solution

Solutions to the exercises of 7 Sketches, Chapter 7: [[7S Chapter 7 Exercises]]. Index: [[Map of Content]].

## Solution 7.4

#proof — [[7S Chapter 7 Exercises#Exercise 7.4|Exercise 7.4]]

Let the diagram be $A \xrightarrow{f} B \xrightarrow{g} C$ over $A' \xrightarrow{f'} B' \xrightarrow{g'} C'$ with verticals $h_1, h_2, h_3$, and suppose the right square is a pullback.

*Left square pullback $\Rightarrow$ rectangle pullback.* Given $p : X \to C$ and $q : X \to A'$ with $q \mathbin{;} f' \mathbin{;} g' = p \mathbin{;} h_3$, the right pullback yields a unique $r : X \to B$ with $r \mathbin{;} h_2 = q \mathbin{;} f'$ and $r \mathbin{;} g = p$; the left pullback then yields a unique $r' : X \to A$ with $r' \mathbin{;} f = r$ and $r' \mathbin{;} h_1 = q$. Hence $r' \mathbin{;} f \mathbin{;} g = p$ and $r' \mathbin{;} h_1 = q$. If $r_0$ also satisfies these, then $r_0 \mathbin{;} f$ satisfies the defining equations of $r$, so $r_0 \mathbin{;} f = r$, and then $r_0 = r'$ by uniqueness in the left square.

*Rectangle pullback $\Rightarrow$ left square pullback.* Given $r : X \to B$ and $q : X \to A'$ with $r \mathbin{;} h_2 = q \mathbin{;} f'$, set $p := r \mathbin{;} g$. Then $p \mathbin{;} h_3 = r \mathbin{;} h_2 \mathbin{;} g' = q \mathbin{;} f' \mathbin{;} g'$, so the rectangle yields a unique $r' : X \to A$ with $r' \mathbin{;} f \mathbin{;} g = p$ and $r' \mathbin{;} h_1 = q$. Now $r' \mathbin{;} f$ and $r$ both satisfy $(-) \mathbin{;} g = p$ and $(-) \mathbin{;} h_2 = q \mathbin{;} f'$, so by uniqueness in the right pullback $r' \mathbin{;} f = r$. Uniqueness of $r'$ follows from uniqueness for the rectangle. $\blacksquare$

> Sources: 7 Sketches, Exercise 7.4 and Solution A.7.

## Solution 7.6

#proof — [[7S Chapter 7 Exercises#Exercise 7.6|Exercise 7.6]]

The pullback definition says: for all $g_1, g_2 : X \to A$, if $g_1 \mathbin{;} f = g_2 \mathbin{;} f$ then $g_1 = g_2$ (the mediating arrow into the pullback $A$ must equal both $g_1$ and $g_2$).
1. If $f$ is mono and $f(a_1) = f(a_2)$, take $X = \{*\}$, $g_i(*) = a_i$. Then $g_1 \mathbin{;} f = g_2 \mathbin{;} f$, so $g_1 = g_2$, so $a_1 = a_2$.
2. If $f$ is injective and $g_1 \mathbin{;} f = g_2 \mathbin{;} f$, then for each $x$, $f(g_1(x)) = f(g_2(x))$ gives $g_1(x) = g_2(x)$; so $g_1 = g_2$.

````tabs
tab: Lean
```lean
import Mathlib
#check @CategoryTheory.mono_iff_injective
```
````

> Sources: 7 Sketches, Exercise 7.6 and Solution A.7.

## Solution 7.7

#proof — [[7S Chapter 7 Exercises#Exercise 7.7|Exercise 7.7]]

1. Let $j = i^{-1}$ and $g := f \mathbin{;} j : A \to B'$. Since $g \mathbin{;} i = f = \mathrm{id}_A \mathbin{;} f$, the universal property gives $j' : A \to A'$ with $j' \mathbin{;} i' = \mathrm{id}_A$ and $j' \mathbin{;} f' = f \mathbin{;} j$. For the other composite: $(i' \mathbin{;} j') \mathbin{;} i' = i'$ and $(i' \mathbin{;} j') \mathbin{;} f' = i' \mathbin{;} f \mathbin{;} j = f' \mathbin{;} i \mathbin{;} j = f'$; $\mathrm{id}_{A'}$ satisfies the same two equations, so by uniqueness $i' \mathbin{;} j' = \mathrm{id}_{A'}$.
2. Given $g : X \to A$ and $h : X \to B$ with $g \mathbin{;} f = h$, the unique $r : X \to A$ with $r \mathbin{;} \mathrm{id}_A = g$ and $r \mathbin{;} f = h$ is $r = g$.

````tabs
tab: Lean
```lean
import Mathlib
open CategoryTheory Limits
#check @CategoryTheory.IsPullback.of_horiz_isIso    -- a square with iso horizontals is a pullback
#check @CategoryTheory.IsPullback.of_id_fst          -- the identity square over f is a pullback
```
````

> Sources: 7 Sketches, Exercise 7.7 and Solution A.7.

## Solution 7.8

#proof — [[7S Chapter 7 Exercises#Exercise 7.8|Exercise 7.8]]

Build a cube: the front and bottom faces are the given pullback, the right face is the square $(A, A, A, B)$ with identities and $f$ — a pullback because $f$ is mono (Definition 7.5) — and the back and top faces are the "identity" squares of [[7S Chapter 7 Exercises#Exercise 7.7|7S Exercise 7.7]] (2), which are pullbacks. By the pasting lemma, right face + back face pullbacks make the diagonal rectangle a pullback; then front face pullback + rectangle pullback make the left face $(A', A', A', B')$ with identities and $f'$ a pullback, which says $f'$ is mono. Hence monomorphisms are stable under pullback.

````tabs
tab: Lean
```lean
import Mathlib
open CategoryTheory Limits
#check @CategoryTheory.Limits.pullback.fst_of_mono    -- Mono g → Mono (pullback.fst f g)
#check @CategoryTheory.Limits.pullback.snd_of_mono    -- Mono f → Mono (pullback.snd f g)
```
````

> Sources: 7 Sketches, Exercise 7.8 and Solution A.7.

## Solution 7.9

#example #program — [[7S Chapter 7 Exercises#Exercise 7.9|Exercise 7.9]]

$\underline{3} \twoheadrightarrow \underline{2} \hookrightarrow \underline{3}$: first the surjection $1, 2 \mapsto 1$, $3 \mapsto 2$ onto the image $\{1, 2\}$, then the inclusion of the image into $\underline{3}$.

````tabs
tab: Julia
```julia
using Catlab
f = FinFunction([1, 1, 2], 3)
e, m = epi_mono(f)
collect(e), collect(m)          # ([1, 1, 2], [1, 2])
compose(e, m) == f              # true
```
````

> Sources: 7 Sketches, Exercise 7.9 and Solution A.7.

## Solution 7.11

#proof — [[7S Chapter 7 Exercises#Exercise 7.11|Exercise 7.11]]

1. $I$ is a top element and $v \otimes w$ satisfies the universal property of the [[Meet]] $v \wedge w$, so the monoidal structure is cartesian. The quantale's $\multimap$ satisfies $v \leq (w \multimap x) \iff v \otimes w \leq x$, which is the universal property of the [[Exponential Object]] $x^w$. So $\mathcal{V}$ is a cartesian closed preorder — a complete [[Heyting Algebra]].
2. No: quantales have all joins, but a cartesian closed preorder need not. Example: $\mathbb{N}^{\mathrm{op}} \times \mathbb{N}^{\mathrm{op}}$ (the [[Product Preorder]], $(a,b) \leq (a',b')$ iff $a' \leq a$ and $b' \leq b$). It has top $(0,0)$, meets $(a,b) \wedge (a',b') = (\max(a,a'), \max(b,b'))$, but no bottom element, hence no empty join. Yet $x \multimap y = \bigvee\{w \mid w \wedge x \leq y\} = \bigvee\{w \mid y \leq w,\ w \wedge x \leq y\}$ exists because only finitely many $w$ lie above $y = (a,b)$ (exactly $(a+1)(b+1)$ of them), and finite nonempty joins are given by componentwise $\min$.

> Sources: 7 Sketches, Exercise 7.11 and Solution A.7.

## Solution 7.16

#example — [[7S Chapter 7 Exercises#Exercise 7.16|Exercise 7.16]]

1. $\mathsf{false}$: $-5 \notin \mathbb{N}$. 2. $\mathsf{true}$: $0 \in \mathbb{N}$.

````tabs
tab: Haskell
```haskell
charN :: Integer -> Bool
charN = (>= 0)          -- charN (-5) == False, charN 0 == True
```
````

> Sources: 7 Sketches, Exercise 7.16 and Solution A.7.

## Solution 7.17

#example — [[7S Chapter 7 Exercises#Exercise 7.17|Exercise 7.17]]

1. Every $n$ is in the image, so $\ulcorner \mathrm{id} \urcorner(n) = \mathsf{true}$ for all $n$ — the predicate $\mathsf{true}$, classifying the top [[Subobject]].
2. Nothing is in the image, so $\ulcorner ! \urcorner(n) = \mathsf{false}$ for all $n$ — the predicate $\mathsf{false}$, classifying the bottom subobject.

> Sources: 7 Sketches, Exercise 7.17 and Solution A.7.

## Solution 7.19

#example — [[7S Chapter 7 Exercises#Exercise 7.19|Exercise 7.19]]

1. A [[Subobject]] of $\mathbb{B}$ — i.e. a subset $A \subseteq \mathbb{B}$ (a mono into $\mathbb{B}$); characteristic maps classify subobjects of their domain ([[Subobject Classifier]]).
2. $A = \{\mathsf{false}\}$: $\neg(b) = \mathsf{true}$ iff $b \in \{\mathsf{false}\}$. See [[Internal Logic of a Topos]].

> Sources: 7 Sketches, Exercise 7.19 and Solution A.7.

## Solution 7.20

#example — [[7S Chapter 7 Exercises#Exercise 7.20|Exercise 7.20]]

1.
| $P$ | $Q$ | $P \wedge Q$ | $P = (P \wedge Q)$ |
|---|---|---|---|
| t | t | t | t |
| t | f | f | f |
| f | t | f | t |
| f | f | f | t |

2. Yes — this is material implication.
3. $\Rightarrow : \mathbb{B} \times \mathbb{B} \to \mathbb{B}$ given by the last column.
4. It classifies $\{(\mathsf{t},\mathsf{t}), (\mathsf{f},\mathsf{t}), (\mathsf{f},\mathsf{f})\} \subseteq \mathbb{B} \times \mathbb{B}$ — the [[Equalizer]] of $\wedge$ and $\pi_1$ ([[Internal Logic of a Topos]]).

> Sources: 7 Sketches, Exercise 7.20 and Solution A.7.

## Solution 7.21

#example #program — [[7S Chapter 7 Exercises#Exercise 7.21|Exercise 7.21]]

1. $\mathsf{false}$. 2. $\mathsf{true}$. 3. $\mathsf{true}$. 4. The set is "even primes or $\geq 10$" $= \{2\} \cup \{10, 11, 12, \dots\}$; smallest three: $2, 10, 11$.

````tabs
tab: Julia
```julia
using Primes
E(n) = iseven(n); P(n) = isprime(n); T(n) = n >= 10
[n for n in 0:20 if (E(n) && P(n)) || T(n)][1:3]      # [2, 10, 11]
```
tab: Haskell
```haskell
isPrime n = n > 1 && all (\d -> n `mod` d /= 0) [2 .. n - 1]
classified = [ n | n <- [0 ..], (even n && isPrime n) || n >= 10 ]   -- take 3 → [2,10,11]
```
````

> Sources: 7 Sketches, Exercise 7.21 and Solution A.7.

## Solution 7.27

#example — [[7S Chapter 7 Exercises#Exercise 7.27|Exercise 7.27]]

1. $B(x, \epsilon) = \{x' \in \mathbb{R} \mid |x - x'| < \epsilon\} = (x - \epsilon, x + \epsilon)$.
2. $U$ is open iff for every $x \in U$ there is $\epsilon > 0$ with $B(x, \epsilon) \subseteq U$ ([[Topological Space]]).
3. $U_1 = (0, 2)$, $U_2 = (1, 3)$ cover $U = (0, 3)$.
4. $U_i = (\tfrac{1}{i}, 1)$ for $i \in \{1, 2, 3, \dots\}$ cover $U = (0, 1)$.

> Sources: 7 Sketches, Exercise 7.27 and Solution A.7.

## Solution 7.29

#proof — [[7S Chapter 7 Exercises#Exercise 7.29|Exercise 7.29]]

1. It contains $X$ and $\varnothing$; $A \cap B = \varnothing$ unless both are $X$; a union is $X$ iff some member is $X$. All three axioms hold.
2. Every subset is open, so every "such-and-such is open" conclusion holds trivially.
3. Continuity requires $f^{-1}(U)$ open in $X$ for each open $U \subseteq Y$; everything in $X$ is open.

````tabs
tab: Lean
```lean
import Mathlib
#check @continuous_of_discreteTopology     -- every map out of a discrete space is continuous
#check @DiscreteTopology
#check @continuous_bot                      -- ⊥ is the discrete topology in Mathlib's order
```
````

> Sources: 7 Sketches, Exercise 7.29 and Solution A.7.

## Solution 7.31

#example — [[7S Chapter 7 Exercises#Exercise 7.31|Exercise 7.31]]

1. $\varnothing \to \{1\} \to \{1, 2\}$, a three-element chain.
2. $(U_i)_{i \in I}$ covers $U$ iff either $I = \varnothing$ and $U = \varnothing$, or $U_i = U$ for some $i$: since the opens are totally ordered, a union equals its largest member. So the only cover not containing $U$ itself is the empty cover of $\varnothing$.

> Sources: 7 Sketches, Exercise 7.31 and Solution A.7.

## Solution 7.32

#proof — [[7S Chapter 7 Exercises#Exercise 7.32|Exercise 7.32]]

1. $Y = X \cap Y$ with $X \in \mathrm{Op}$.
2. $\varnothing = \varnothing \cap Y$. If $A_i = B_i \cap Y$ then $A_1 \cap A_2 = (B_1 \cap B_2) \cap Y$ and $\bigcup_i A_i = (\bigcup_i B_i) \cap Y$, and $B_1 \cap B_2$, $\bigcup_i B_i$ are open in $X$.
3. The preimage of $B \in \mathrm{Op}$ under the inclusion is $B \cap Y$, which is open by definition.

````tabs
tab: Lean
```lean
import Mathlib
#check @instTopologicalSpaceSubtype
#check @isOpen_induced_iff              -- IsOpen s ↔ ∃ t, IsOpen t ∧ f ⁻¹' t = s
#check @continuous_subtype_val
```
````

> Sources: 7 Sketches, Exercise 7.32 and Solution A.7.

## Solution 7.34

#annotation — [[7S Chapter 7 Exercises#Exercise 7.34|Exercise 7.34]]

A $\mathcal{V}$-category has objects and, for each pair $a, b$, an open set $\mathcal{C}(a, b) \subseteq X$, with $X \subseteq \mathcal{C}(a, a)$ and $\mathcal{C}(a, b) \cap \mathcal{C}(b, c) \subseteq \mathcal{C}(a, c)$. Think of $\mathcal{C}(a, b)$ as a *size restriction* for getting from $a$ to $b$ — bridges your truck must fit under. Going from $a$ to itself has no restriction ($X$). Along a path you must fit under every bridge (meet $= \cap$), and you may take any path (join $= \cup$): as in [[Matrix Multiplication in a Quantale]], $\mathcal{C}(B, C) = (U_3 \cap U_1) \cup (U_4 \cap U_2)$ for the two paths of the book's example.

> Sources: 7 Sketches, Exercise 7.34 and Solution A.7.

## Solution 7.38

#example — [[7S Chapter 7 Exercises#Exercise 7.38|Exercise 7.38]]

1. $\{a_1, a_2\}$. 2. $\{c_1\}$. 3. $\varnothing$. 4. E.g. send $a_2, e_1 \mapsto a$; $c_1, b_1 \mapsto b$; $b_2 \mapsto c$; $b_3 \mapsto d$; $e_2, a_1 \mapsto e$ (eight elements over five points, fibers of sizes $2, 2, 1, 1, 2$).

> Sources: 7 Sketches, Exercise 7.38 and Solution A.7.

## Solution 7.40

#example #program — [[7S Chapter 7 Exercises#Exercise 7.40|Exercise 7.40]]

1. Six sections $(a_i, b_j, c_1)$ for $i \in \{1,2\}$, $j \in \{1,2,3\}$.
2. None: the fiber over $d$ is empty, so $\mathrm{Sec}_f(V_2) = \varnothing$.
3. Also none, for the same reason — $|\mathrm{Sec}_f(V_3)| = 2 \cdot 3 \cdot 0 \cdot 2 = 0$. (The printed solution says $12$, forgetting the empty fiber over $d$.)

````tabs
tab: Julia
```julia
fibers = Dict("a"=>2, "b"=>3, "c"=>1, "d"=>0, "e"=>2)
nsections(U) = prod(fibers[u] for u in U)
nsections(["a","b","c"]), nsections(["a","b","c","d"]), nsections(["a","b","d","e"])   # (6, 0, 0)
```
````

> Sources: 7 Sketches, Exercise 7.40 and Solution A.7.

## Solution 7.42

#example — [[7S Chapter 7 Exercises#Exercise 7.42|Exercise 7.42]]

1. $\mathrm{Sec}_f(\{a,b,c\}) = \{(a_1,b_1,c_1), (a_1,b_2,c_1), (a_1,b_3,c_1), (a_2,b_1,c_1), (a_2,b_2,c_1), (a_2,b_3,c_1)\}$; $\mathrm{Sec}_f(\{a,c\}) = \{(a_1, c_1), (a_2, c_1)\}$.
2. Restriction drops the $b$-component: the first three sections go to $(a_1, c_1)$, the last three to $(a_2, c_1)$.

> Sources: 7 Sketches, Exercise 7.42 and Solution A.7.

## Solution 7.44

#example — [[7S Chapter 7 Exercises#Exercise 7.44|Exercise 7.44]]

1. $s_1 = (a_1, b_1)$, $s_2 = (b_2, e_1)$.
2. No: a section over $\{a, b, e\}$ has a single $b$-value, which would have to be both $b_1$ and $b_2$.
3. $h_1 = (a_2, b_3)$, $h_2 = (b_3, e_2)$.
4. Yes, uniquely: $h = (a_2, b_3, e_2)$. This is the [[Sheaf|sheaf condition]] in action.

> Sources: 7 Sketches, Exercise 7.44 and Solution A.7.

## Solution 7.47

#annotation — [[7S Chapter 7 Exercises#Exercise 7.47|Exercise 7.47]]

No. The set of *all* vector fields (over all opens) forms *one* sheaf, $\mathrm{Sec}_\pi$ for the tangent bundle $\pi : TM \to M$ ([[Sheaf of Sections]]). The sheaves on $M$ do not even form a set — they form a [[Topos]] $\mathbf{Shv}(M)$ — and $\mathrm{Sec}_\pi$ is one object of it.

> Sources: 7 Sketches, Exercise 7.47 and Solution A.7.

## Solution 7.49

#example — [[7S Chapter 7 Exercises#Exercise 7.49|Exercise 7.49]]

1. The chain $\varnothing \to \{1\} \to \{1,2\}$.
2. Three sets and two functions $F(\{1,2\}) \to F(\{1\}) \to F(\varnothing)$.
3. The only non-trivial cover is the empty cover of $\varnothing$ ([[7S Chapter 7 Exercises#Exercise 7.31|7S Exercise 7.31]]), whose sheaf condition is $F(\varnothing) = \{()\}$ (Example 7.36).
4. Hence a sheaf is a set $F(\{1,2\})$, a set $F(\{1\})$ and a function between them; $\mathbf{Shv}(\text{Sierpiński})$ is equivalent to the arrow category $\mathbf{Set}^{\to}$.

> Sources: 7 Sketches, Exercise 7.49 and Solution A.7.

## Solution 7.52

#example — [[7S Chapter 7 Exercises#Exercise 7.52|Exercise 7.52]]

$X = \{1\}$ has opens $\varnothing$ and $\{1\}$; a sheaf $S$ on it has $S(\varnothing) = \{()\}$ forced, so its only data is the set $S(\{1\})$ — this is the identification $\mathbf{Set} \simeq \mathbf{Shv}(\{1\})$. Now $\Omega(\{1\}) = \{\varnothing, \{1\}\}$, a two-element set, corresponding to $\mathsf{false}$ and $\mathsf{true}$.

> Sources: 7 Sketches, Exercise 7.52 and Solution A.7.

## Solution 7.53

#proof — [[7S Chapter 7 Exercises#Exercise 7.53|Exercise 7.53]]

1. For $W \subseteq V \subseteq U$: $(U' \cap V) \cap W = U' \cap W$ since $W \subseteq V$; identities: $U' \cap U = U'$ for $U' \subseteq U$.
2. Yes: a presheaf is just a functor $\mathrm{Op}^{\mathrm{op}} \to \mathbf{Set}$, and functoriality is all there is to check. (That it is moreover a [[Sheaf]] — the [[Subobject Classifier]] of $\mathbf{Shv}(X)$ — is verified separately.)

> Sources: 7 Sketches, Exercise 7.53 and Solution A.7.

## Solution 7.55

#example #program — [[7S Chapter 7 Exercises#Exercise 7.55|Exercise 7.55]]

Write $\gamma = \ulcorner H \urcorner$. Vertices: $\gamma(A) = \gamma(B) = \gamma(C) = V$ (present), $\gamma(D) = 0$ (missing). Arrows: $\gamma(f) = (V, V; A)$ (present); $\gamma(g) = \gamma(h) = (V, V; 0)$ (endpoints present, arrow missing); $\gamma(i) = (V, 0; 0)$ (source present, target missing). This is the unique homomorphism whose pullback of $\mathsf{true}$ is $H$.

````tabs
tab: Julia
```julia
using Catlab
Ω, _ = subobject_classifier(Graph)     # vertex 1 = V, 2 = 0; edges 1=(V,V;A) 2=(V,V;0) 3=(V,0;0) 4=(0,V;0) 5=(0,0;0)
G = @acset Graph begin V = 4; E = 4; src = [1, 1, 2, 3]; tgt = [2, 2, 3, 4] end   # A,B,C,D; f,g,h,i
γ = ACSetTransformation(G, Ω; V=[1, 1, 1, 2], E=[1, 2, 2, 3])
is_natural(γ)                           # true
```
````

> Sources: 7 Sketches, Exercise 7.55 and Solution A.7.

## Solution 7.59

#example — [[7S Chapter 7 Exercises#Exercise 7.59|Exercise 7.59]]

1. $\neg U$ is the interior of the complement $\{0\}$, which is $\varnothing$.
2. $\neg\neg U$ is the interior of $\mathbb{R} \setminus \varnothing = \mathbb{R}$, i.e. $\mathbb{R}$.
3. Yes. 4. No: $0 \in \neg\neg U$ but $0 \notin U$. Double negation is not the identity — the logic of a sheaf topos is intuitionistic.

> Sources: 7 Sketches, Exercise 7.59 and Solution A.7.

## Solution 7.60

#proof — [[7S Chapter 7 Exercises#Exercise 7.60|Exercise 7.60]]

1. Taking $U = X$: $\top \cap X = X$, and $\top \cap X = \top$, so $\top = X$.
2. $X \cup U = X$; $U \Rightarrow X = \bigcup\{R \mid R \cap U \subseteq X\} = X$; $X \Rightarrow U = \bigcup\{R \mid R \subseteq U\} = U$.
3. Taking $U = \varnothing$: $\bot \cup \varnothing = \varnothing$ so $\bot = \varnothing$.
4. $\varnothing \cap U = \varnothing$; $\varnothing \Rightarrow U = \bigcup\{R \mid R \cap \varnothing \subseteq U\} = X$.

> Sources: 7 Sketches, Exercise 7.60 and Solution A.7.

## Solution 7.62

#example — [[7S Chapter 7 Exercises#Exercise 7.62|Exercise 7.62]]

If $S$ is the sheaf of people (a section over an interval $U$ is a person alive throughout $U$), then a section of $\{S \mid p\}$ over $U$ is a person alive throughout $U$ who likes the weather throughout $U$ — i.e. a section $s \in S(U)$ with $p(s) = U$.

> Sources: 7 Sketches, Exercise 7.62 and Solution A.7.

## Solution 7.64

#example — [[7S Chapter 7 Exercises#Exercise 7.64|Exercise 7.64]]

Formal: $X$ the one-point space, $S = \mathbb{N}$, $p(s)$ = "$24 \leq s \leq 28$", $q(s)$ = "$s$ is not prime". Informal: $X$ the surface of the Earth, $S$ the sheaf of wind vector fields, $p$ = "wind blows due east at 2–5 km/h", $q$ = "wind blows at 1–5 km/h"; wherever $p$ holds, $q$ holds.

> Sources: 7 Sketches, Exercise 7.64 and Solution A.7.

## Solution 7.66

#example #program — [[7S Chapter 7 Exercises#Exercise 7.66|Exercise 7.66]]

1. $\{0\}$ (only $0 \leq |z|$ for all $z$, since $z = 0$ must work). 2. All of $\mathbb{N}$ (take $z = n$). 3. $\varnothing$ (no $z$ exceeds every $n$). 4. All of $\mathbb{Z}$ (take $n = 0$).

````tabs
tab: Haskell
```haskell
p :: Integer -> Integer -> Bool
p n z = n <= abs z
-- on finite windows: [n | n <- [0..5], all (p n) [-9..9]] == [0]
--                    [z | z <- [-9..9], all (`p` z) [0..20]] == []
```
````

> Sources: 7 Sketches, Exercise 7.66 and Solution A.7.

## Solution 7.67

#example — [[7S Chapter 7 Exercises#Exercise 7.67|Exercise 7.67]]

1. The largest open $V \subseteq U$ such that $p(s|_V, t) = V$ for every $t \in T(V)$: the largest interval throughout which $s$ is worried about *every* item in the news throughout that interval. For most people this is empty (there is always a happy kitten somewhere).
2. Yes, exactly.

> Sources: 7 Sketches, Exercise 7.67 and Solution A.7.

## Solution 7.68

#example — [[7S Chapter 7 Exercises#Exercise 7.68|Exercise 7.68]]

1. The union of all intervals $V_i \subseteq U$ for which some news item $t_i \in T(V_i)$ worries $s$ throughout $V_i$: all the time during which $s$ is worried about *at least one* thing — the thing being allowed to change.
2. Reasonable: "such a string of bad news, it's like I'm always worried about something". Someone who wants a *single* item to worry $s$ throughout is working in a different topos, with fewer coverings — it is the notion of covering that makes $\exists$ behave this way.

> Sources: 7 Sketches, Exercise 7.68 and Solution A.7.

## Solution 7.70

#proof — [[7S Chapter 7 Exercises#Exercise 7.70|Exercise 7.70]]

($\Leftarrow$) by reflexivity. ($\Rightarrow$) With $p := j(q)$ we have $j(p) \leq p$ by hypothesis and $p \leq j(p)$ by inflation; $\Omega(U)$ is a [[Partial Order|poset]], so $p = j(p)$, i.e. $j(q) = j(j(q))$.

> Sources: 7 Sketches, Exercise 7.70 and Solution A.7.

## Solution 7.72

#example — [[7S Chapter 7 Exercises#Exercise 7.72|Exercise 7.72]]

1. $p(s)$ = "$s$ likes the weather".
2. $U$ = January 2019; $p(s) \subseteq U$ is the sub-interval throughout which $s$ likes the weather.
3. $j(p(s)) = (\text{Bob in SD}) \Rightarrow p(s)$: the times at which either Bob is not in San Diego or $s$ likes the weather.
4. Yes, by 3.
5. Yes: "if Bob is in SD then (if Bob is in SD then $p$)" is equivalent to "if Bob is in SD then $p$".
6. Yes, with $q$ = "$s$ is happy": "if Bob is in SD then ($p$ and $q$)" iff ("if Bob is in SD then $p$" and "if Bob is in SD then $q$").

> Sources: 7 Sketches, Exercise 7.72 and Solution A.7.

## Solution 7.76

#example — [[7S Chapter 7 Exercises#Exercise 7.76|Exercise 7.76]]

1. $0 < 2 \leq 6 < 8$.
2. $[2,6] \in o_{[0,5]}$ would need $6 < 5$; $[2,6] \in o_{[4,8]}$ would need $4 < 2$. Neither holds.

> Sources: 7 Sketches, Exercise 7.76 and Solution A.7.

## Solution 7.77

#proof — [[7S Chapter 7 Exercises#Exercise 7.77|Exercise 7.77]]

Since $\mathbb{R} = \{[x, x]\}$, $o_{[a,b]} \cap \mathbb{R} = \{x \mid a < x < b\} = B(\tfrac{a+b}{2}, \tfrac{b-a}{2})$. If $U = U' \cap \mathbb{R}$ with $U' = \bigcup_i o_{[a_i, b_i]}$, then $U = \bigcup_i (a_i, b_i)$ is a union of open balls, hence open. Conversely if $U = \bigcup_j B(m_j, \epsilon_j)$, put $a_j = m_j - \epsilon_j$, $b_j = m_j + \epsilon_j$; then $U = \big(\bigcup_j o_{[a_j, b_j]}\big) \cap \mathbb{R}$ is open in the subspace topology.

> Sources: 7 Sketches, Exercise 7.77 and Solution A.7.

## Solution 7.80

#proof — [[7S Chapter 7 Exercises#Exercise 7.80|Exercise 7.80]]

1. Yes: for $V \subseteq U$ restrict $f$ along $V \cap R \subseteq U \cap R$; this is functorial.
2. Yes: given a cover $U = \bigcup_i U_i$ and continuous $f_i : U_i \cap R \to X$ agreeing on overlaps, they glue to a unique continuous function on $U \cap R = \bigcup_i (U_i \cap R)$ (continuity is local). So $H_X \in \mathbf{Shv}(\mathbb{I}\mathbb{R})$ — a [[Topos of Behavior Types|behavior type]]; with $R = \mathbb{R}$ this is $G_X$ of Example 7.79.

> Sources: 7 Sketches, Exercise 7.80 and Solution A.7.
