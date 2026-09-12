#definition #theorem #proof

A **universal arrow from $L$ to $c$** (for a functor $L : \mathcal{D} \to \mathcal{C}$ and object $c$) is a [[Terminal Object]] $(t, \tau : Lt \to c)$ in the [[Comma Category]] $L \downarrow c$: for every $f : Ld \to c$ there is a unique $h : d \to t$ with $f = \tau \circ Lh$. Equivalently, the map $h \mapsto \tau \circ Lh$ is a bijection $\mathcal{D}(d, t) \cong \mathcal{C}(Ld, c)$. Dually a universal arrow from $c$ to $R$ is an initial object $(t, \eta : c \to Rt)$ in $c \downarrow R$.

> Sources: DaoFP §10.6 ("Adjunctions Using Universal Arrows"), §10.8; 7 Sketches Definition 3.86/3.92 (products and limits are universal cones), §3.6 ("universal constructions"); Kittenlab Lecture 8, 11 (universal objects "with these mappings").

**Theorem (DaoFP).** (1) If $L \dashv R$ then for every $c$, $(Rc, \varepsilon_c)$ is a universal arrow from $L$ to $c$. (2) Conversely, if every $c$ has a universal arrow $(t_c, \varepsilon_c)$ from $L$, then $Rc := t_c$ extends to a functor right adjoint to $L$, and the $\varepsilon_c$ are automatically natural and form the counit.

*Proof of (1).* Naturality of $\phi_{d,c} : \mathcal{C}(Ld, c) \to \mathcal{D}(d, Rc)$ in $d$ gives, for $h : d \to d'$, the square $\phi_{d,c}(f' \circ Lh) = \phi_{d',c}(f') \circ h$. Apply the Yoneda trick with $d' := Rc$ and the identity $\mathrm{id}_{Rc}$: the upper-left corner becomes $\varepsilon_c = \phi^{-1}(\mathrm{id}_{Rc})$, the lower-right $h$, the upper-right its mate $f$, so $f = \varepsilon_c \circ Lh$; uniqueness because $\phi$ is a bijection. $\blacksquare$

The unit and counit of an [[Adjunction]] are thus its universal arrows ([[Unit and Counit of an Adjunction]]). [[Limit|Limits]] are universal cones (terminal in the cone category), [[Colimit|colimits]] universal cocones, [[Exponential Object|evaluation]] $\varepsilon : b^a \times a \to b$ is the universal arrow from $(- \times a)$ to $b$, and the [[Free Monoid|free monoid]]'s insertion of generators is the universal arrow from a set to the forgetful functor. Kittenlab's "coequalizing morphism" $e : B \to C$ is the universal arrow characterizing the [[Coequalizer]] without carrying a whole natural isomorphism around.

````tabs
tab: Lean
```lean
#check CategoryTheory.Adjunction.adjunctionOfEquivLeft    -- adjunction from a family of universal arrows
#check CategoryTheory.CostructuredArrow                    -- the comma category L ↓ c
#check CategoryTheory.Limits.IsTerminal                    -- a universal arrow is a terminal object in it
```
````
