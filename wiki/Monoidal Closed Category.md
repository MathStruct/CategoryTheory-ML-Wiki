#definition #example

A [[Monoidal Category]] $(\mathcal{C}, \otimes, I)$ is **monoidal closed** (**closed**) if for every pair of objects $c, d$ there is an object $c \multimap d$ (the **internal hom**, also $[c, d]$ or $d^c$) with a natural isomorphism
$$\mathcal{C}(b \otimes c, d) \cong \mathcal{C}(b, c \multimap d),$$
i.e. $(- \otimes c) \dashv (c \multimap -)$ for every $c$ — the [[Categorification|categorification]] of a [[Monoidal Closed Preorder]]. The counit $\varepsilon : [c, d] \otimes c \to d$ is **evaluation**, the unit $b \to [c, b \otimes c]$ **coevaluation**.

> Sources: 7 Sketches §4.5.1 (text before Proposition 4.60), Remark 2.81, Definition 2.79; DaoFP §10.1 (internal vs. external hom), §19.1 ("Closed Monoidal Categories", "Internal hom for Day convolution", "Powering and co-powering"), §20.1 ("Self-enrichment"), §20.2.

- [[Cartesian Closed Category|Cartesian closed]] categories are the case $\otimes = \times$ ([[Exponential Object]]); [[Compact Closed Category|compact closed]] categories are the case where $c \multimap d = c^* \otimes d$ (Proposition 4.60). Every monoidal closed category is self-[[Enriched Category|enriched]] via $[a, b]$, with composition built from evaluation (DaoFP §20.1), and its [[Hom Functor]] is an [[Enriched Functor]].
- **Examples**: $\mathbf{Set}$, $\mathbf{Cat}$ (with $[\mathcal{C}, \mathcal{D}]$), $\mathbf{Vect}_k$ with $\mathrm{Hom}_k(V, W)$, [[Category of Profunctors|$\mathbf{Prof}$]], $[\mathcal{C}, \mathbf{Set}]$ with [[Day Convolution]] and its internal hom $[F, G](a) = \int_x [F x, G(a \otimes x)]$ (DaoFP §19.1); the preorder cases $\mathbf{Bool}$ ($\Rightarrow$), $\mathbf{Cost}$ (truncated subtraction), $\mathcal{P}(S)$.
- $\mathbf{Set}$-valued functors on a closed $\mathcal{V}$ are enriched co-presheaves $\mathcal{C} \to \mathcal{V}$ with $F_{ab} : \mathcal{C}(a,b) \to [Fa, Fb]$; **powering and copowering** $a \pitchfork v$, $v \cdot a$ generalize exponentials and tensors by a set (DaoFP §19.1).
- Internal homs make [[Functorial Strength|strong]] functors and [[Enriched Functor|enriched functors]] coincide in a closed category: every Haskell `Functor` is both.

````tabs
tab: Lean
```lean
#check CategoryTheory.MonoidalClosed        -- class: every object X has a `Closed X` structure, i.e. tensorLeft X ⊣ ihom X
#check CategoryTheory.ihom                   -- the internal hom functor
#check CategoryTheory.ihom.ev                -- evaluation
#check CategoryTheory.ihom.coev              -- coevaluation
```
tab: Haskell
```haskell
-- in Hask the internal hom is (->): eval and coeval
eval :: (a -> b, a) -> b
eval (f, a) = f a
coeval :: b -> (a -> (b, a))
coeval b = \a -> (b, a)
-- DaoFP §20.2: strength from enrichment, `strength (a, fb) = fmap (a,) fb`
```
````
