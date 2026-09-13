#definition #theorem #proof #example

Given an [[Adjunction]] $L \dashv R$ with hom-set isomorphism $\phi_{x,y} : \mathcal{D}(Lx, y) \cong \mathcal{C}(x, Ry)$:

- setting $y := Lx$ and applying $\phi$ to $\mathrm{id}_{Lx}$ (the *Yoneda trick*) gives the **unit** $\eta_x : x \to R(Lx)$, a [[Natural Transformation]] $\eta : \mathrm{Id} \Rightarrow R \circ L$;
- setting $x := Ry$ and applying $\phi^{-1}$ to $\mathrm{id}_{Ry}$ gives the **counit** $\varepsilon_y : L(Ry) \to y$, a natural transformation $\varepsilon : L \circ R \Rightarrow \mathrm{Id}$.

They satisfy the **triangle identities** (zig-zag identities), using horizontal composition/whiskering:

$$
(\varepsilon \circ L) \cdot (L \circ \eta) = \mathrm{id}_L, \qquad (R \circ \varepsilon) \cdot (\eta \circ R) = \mathrm{id}_R,
$$

i.e. $L \xrightarrow{L\eta} LRL \xrightarrow{\varepsilon L} L$ and $R \xrightarrow{\eta R} RLR \xrightarrow{R \varepsilon} R$ are identities. Conversely, natural transformations $\eta, \varepsilon$ satisfying the triangle identities determine the adjunction: $f : x \to Ry$ has mate $\varepsilon_y \circ Lf$, and $g : Lx \to y$ has mate $Rg \circ \eta_x$ ([[DaoFP Chapter 10 Exercises#Exercise 10.5.2|DaoFP Exercise 10.5.2]]). "$\eta$ can be used to insert $RL$ anywhere an identity would work; $\varepsilon$ to eliminate $LR$."

> Sources: DaoFP §10.5 ("Unit and Counit of an Adjunction", "Triangle identities", "The unit and counit of the currying adjunction"), Exercises 10.5.1–10.5.4; 7 Sketches Proposition 1.107 (preorder version: $p \leq g(f(p))$ and $f(g(q)) \leq q$), Exercise 1.119; Kittenlab Lecture 7 ($\eta_X : X \to UF X$, $x \mapsto [x]$).

## Examples

| adjunction | unit | counit |
|---|---|---|
| $(+) \dashv \Delta$ | the pair of injections $\langle \mathsf{Left}, \mathsf{Right} \rangle : (a, b) \to \Delta(a+b)$ | $[\mathrm{id}, \mathrm{id}] : x + x \to x$ ([[DaoFP Chapter 10 Exercises#Exercise 10.5.1|DaoFP Exercise 10.5.1]]) |
| $\Delta \dashv (\times)$ | $\langle \mathrm{id}, \mathrm{id} \rangle : x \to x \times x$ | the pair of projections $\langle \mathsf{fst}, \mathsf{snd} \rangle$ |
| $(- \times a) \dashv (-)^a$ | $\eta : e \to (e \times a)^a$, `unit = curry id` (the curried pair constructor) | $\varepsilon : b^a \times a \to b$, `counit = uncurry id` — function application |
| free $\dashv$ forgetful monoid | $x \mapsto [x]$, singleton list | $\mathrm{fold}$: evaluate a list of monoid elements ([[DaoFP Chapter 10 Exercises#Exercise 10.9.1|DaoFP Exercise 10.9.1]]) |
| [[Galois Connection]] $f \dashv g$ | $p \leq g(f(p))$ | $f(g(q)) \leq q$ |
| $\mathrm{Colim} \dashv \Delta \dashv \mathrm{Lim}$ | the colimit cocone / the map into the limit of a constant diagram | the universal cocone map / the limit cone |

In [[String Diagram|string diagrams]] (DaoFP §15.1) the unit is a cup and the counit a cap, and the triangle identities say a zig-zag string straightens out. The unit and counit are the [[Universal Arrow|universal arrows]] of the adjunction (DaoFP §10.6). The composite $RL$ with $\eta$ and $R \varepsilon L$ is a [[Monad]]; $LR$ with $\varepsilon$ and $L \eta R$ a [[Comonad]]. In a preorder, $g \circ f$ is a [[Closure Operator]].

````tabs
tab: Lean
```lean
#check CategoryTheory.Adjunction.unit          -- 𝟭 C ⟶ F ⋙ G
#check CategoryTheory.Adjunction.counit        -- G ⋙ F ⟶ 𝟭 D
#check CategoryTheory.Adjunction.left_triangle_components
#check CategoryTheory.Adjunction.right_triangle_components
#check CategoryTheory.Adjunction.mkOfUnitCounit
```
tab: Haskell
```haskell
-- DaoFP §10.5: unit and counit of currying
unit :: e -> (a -> (e, a))
unit = curry id

counit :: (a -> b, a) -> b
counit = uncurry id          -- function application

-- triangle identity tests (DaoFP Exercise 10.5.3/10.5.4)
-- triangle (L (2, 'a')) == L (2, 'a');   let R f = triangle' (R (+1)) in f 3 == 4
```
````
