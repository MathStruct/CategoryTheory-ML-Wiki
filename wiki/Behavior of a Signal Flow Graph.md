#definition #example

The **behavior** of a [[Signal Flow Graph]] $g : m \to n$ over a [[Rig]] $R$ is the set of input/output pairs

$$
B(g) := \{(x, S(g)(x)) \mid x \in R^m\} \subseteq R^m \times R^n,
$$

the graph of the linear map given by the matrix $S(g)$ (Eq. 5.75). For the copy icon, $B = \{(x, (x, x))\}$. The **mirror image** $g^{\mathrm{op}} : n \to m$ of an icon has the **transposed relation** $B(g^{\mathrm{op}}) := \{(S(g)(x), x)\}$ (Eq. 5.76). Behaviours compose as relations: $B_1 \mathbin{;} B_2 = \{(x,z) \mid \exists y.\ (x,y) \in B_1, (y,z) \in B_2\}$ (Eq. 5.78), giving a prop functor $B : \mathbf{SFG}^+_R \to \mathbf{Rel}_R$ on non-simplified signal flow graphs (Eq. 5.81).

> Sources: 7 Sketches §5.4.3 ("The behavioral approach", "Mirror image of an icon", "Combining directions"), Exercises 5.77, 5.80, 5.82–5.85, Theorem 5.87; Willems' behavioural approach [Wil07]; Kittenlab Lecture 14 (a model as an exclusion law: a subset of a universum).

- Reversed add: $\{(x, (y,z)) \mid x = y + z\}$; reversed copy: $\{((y, z), x) \mid x = y = z\}$ ([[7S Exercise 5.77]]).
- $B(g \mathbin{;} h^{\mathrm{op}}) = \{(x, y) \mid S(g)x = S(h)y\}$ and $B(g^{\mathrm{op}} \mathbin{;} h) = \{(S(g)x, S(h)x)\}$ ([[7S Exercise 5.82]], [[7S Exercise 5.83]]): systems of linear equations and parametrized subspaces.
- Behaviours are linear relations; kernel = compose with reversed zeros, image = compose with reversed discards ([[7S Exercise 5.84]]).
- Cup and cap behaviours $\{(0, (x,x))\}$, $\{((x,x), 0)\}$ make $\mathbf{Rel}_R$ [[Compact Closed Category|compact closed]] (Theorem 5.87). See [[Graphical Linear Algebra]].
