#exercise #solution #proof

**Exercise 7.11.** Let $\mathcal{V} = (V, \leq, I, \otimes)$ be a [[Quantale]] with $v \leq I$, $v \otimes w \leq v$ and $v \otimes w \leq w$, and ($x \leq v$ and $x \leq w$) $\Rightarrow x \leq v \otimes w$. 1. Show $\mathcal{V}$ is a [[Cartesian Closed Category|cartesian closed]] preorder. 2. Can every cartesian closed preorder be obtained this way?

## Solution

1. $I$ is a top element and $v \otimes w$ satisfies the universal property of the [[Meet]] $v \wedge w$, so the monoidal structure is cartesian. The quantale's $\multimap$ satisfies $v \leq (w \multimap x) \iff v \otimes w \leq x$, which is the universal property of the [[Exponential Object]] $x^w$. So $\mathcal{V}$ is a cartesian closed preorder — a complete [[Heyting Algebra]].
2. No: quantales have all joins, but a cartesian closed preorder need not. Example: $\mathbb{N}^{\mathrm{op}} \times \mathbb{N}^{\mathrm{op}}$ (the [[Product Preorder]], $(a,b) \leq (a',b')$ iff $a' \leq a$ and $b' \leq b$). It has top $(0,0)$, meets $(a,b) \wedge (a',b') = (\max(a,a'), \max(b,b'))$, but no bottom element, hence no empty join. Yet $x \multimap y = \bigvee\{w \mid w \wedge x \leq y\} = \bigvee\{w \mid y \leq w,\ w \wedge x \leq y\}$ exists because only finitely many $w$ lie above $y = (a,b)$ (exactly $(a+1)(b+1)$ of them), and finite nonempty joins are given by componentwise $\min$.

> Sources: 7 Sketches, Exercise 7.11 and Solution A.7.
