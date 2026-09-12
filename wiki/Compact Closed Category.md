#definition #example #theorem #proof

Let $(\mathcal{C}, I, \otimes)$ be a [[Symmetric Monoidal Category]] and $c$ an object. A **dual** for $c$ consists of

(i) an object $c^*$, the **dual** of $c$;
(ii) a morphism $\eta_c : I \to c^* \otimes c$, the **unit** (drawn as a **cup**);
(iii) a morphism $\varepsilon_c : c \otimes c^* \to I$, the **counit** (a **cap**),

satisfying the **snake equations** (zig-zag identities):
$$c \cong c \otimes I \xrightarrow{c \otimes \eta_c} c \otimes (c^* \otimes c) \cong (c \otimes c^*) \otimes c \xrightarrow{\varepsilon_c \otimes c} I \otimes c \cong c \quad\text{is } \mathrm{id}_c,$$
and symmetrically $c^* \to c^* \otimes c \otimes c^* \to c^*$ is $\mathrm{id}_{c^*}$. If every object has a dual, $\mathcal{C}$ is **compact closed**.

> Sources: 7 Sketches §4.5 (Definition 4.58, Eq. 4.59, Proposition 4.60, Examples 4.61, Theorem 4.63, Exercises 4.62, 4.64–4.66), §4.1 (Eq. 4.1), §6.6; DaoFP §15.1 (string diagrams: cups and caps for adjunctions), §19.1; [Sel10].

## Wiring diagrams with feedback

In a compact closed category wires carry a **direction**: a forward wire labelled $c$ equals a backward wire labelled $c^*$. The cup $\eta_c$ and cap $\varepsilon_c$ let any wire reverse direction, so outputs can be bent into inputs — the feedback loops of [[Co-design]] diagrams (Eq. 4.1: the chassis carries the weight of the motor that powers it) and of Eq. 4.55–4.57 ("Person 1" emits sound and fury, "Person 2" receives sound and emits a complaint; bending the fury wire back). The snake equations say a zig-zag wire can be straightened. "This same structure shows up in quantum mechanics and dynamical systems."

```tikz
\usepackage{tikz}
\begin{document}
\begin{tikzpicture}
\draw (0,0) -- (1,0) arc (90:-90:0.4) -- (0,-0.8) node[left]{$c^*$};
\node[left] at (0,0) {$c$};
\node at (0.7,-1.4) {$\varepsilon_c$ (cap)};
\draw (3,0) node[right]{$c^*$} -- (2.5,0) arc (90:270:0.4) -- (3,-0.8) node[right]{$c$};
\node at (2.6,-1.4) {$\eta_c$ (cup)};
\draw (5,0) -- (6,0) arc (90:-90:0.3) -- (5.5,-0.6) arc (90:270:0.3) -- (7,-1.2);
\node at (7.4,-0.6) {$=$};
\draw (7.8,-0.6) -- (9,-0.6);
\node at (7,-1.8) {snake equation};
\end{tikzpicture}
\end{document}
```

## Examples

- **[[Category of Profunctors|$\mathbf{Prof}_{\mathcal{V}}$]] and $\mathbf{Feas}$** (Theorem 4.63): $\otimes$ is the [[Product of Enriched Categories|product of $\mathcal{V}$-categories]], $I = \mathbf{1}$, $\mathcal{X}^* = \mathcal{X}^{\mathrm{op}}$, and $\eta_{\mathcal{X}}(1, x, x') = \varepsilon_{\mathcal{X}}(x, x', 1) = \mathcal{X}(x, x')$ — "the unit and counit look like identities" ([[7S Exercise 4.66]]). This is what "puts actual mathematics behind" co-design diagrams: a resource required can be read as a resource provided in the opposite preorder.
- **[[Corelation|$\mathbf{Corel}$]]** (Example 4.61): finite sets and corelations (equivalence relations on $A \sqcup B$), monoidal under $\sqcup$, every set self-dual with $\eta_A : \varnothing \to A \sqcup A$ and $\varepsilon_A : A \sqcup A \to \varnothing$ both the "pairing" equivalence relation $\{(a,1),(a,2)\}$ ([[7S Exercise 4.62]]).
- $\mathbf{FinVect}_k$ with $V^*$ the dual space, $\eta$ the identity tensor $\sum e_i^* \otimes e_i$ and $\varepsilon$ evaluation — the origin of the name; [[Category of Relations|$\mathbf{Rel}$]] with $\sqcup$ or $\times$; [[Cospan|$\mathbf{Cosp}$]] and every [[Hypergraph Category]] (each object self-dual via the Frobenius cup and cap).
- $\mathbf{Set}$ with $\times$ is *not* compact closed (a set with more than one element has no dual), though it is [[Cartesian Closed Category|cartesian closed]].

## Proposition 4.60

If $\mathcal{C}$ is compact closed then (1) $\mathcal{C}$ is [[Monoidal Closed Category|monoidal closed]] with $c \multimap d := c^* \otimes d$ and the isomorphism $\mathcal{C}(b \otimes c, d) \cong \mathcal{C}(b, c^* \otimes d)$ given by precomposing with $\mathrm{id}_b \otimes \eta_c$ (and using the snake equations for the inverse); (2) duals are unique up to isomorphism; (3) $c \cong c^{**}$. Compact closed categories are thus a special kind of closed monoidal category, hence the name; in $\mathbf{Feas}$ the internal hom $X^{\mathrm{op}} \times Y$ is the preorder whose elements are exactly the pairs in a feasibility relation. DaoFP's [[String Diagram|string diagrams]] for [[Adjunction|adjunctions]] use the same cups/caps: an adjunction is a "dual pair" of 1-cells in the 2-category $\mathbf{Cat}$.

````tabs
tab: Julia
```julia
# Catlab: the GAT of compact closed categories, with duals, units (cups) and counits (caps)
using Catlab
@present CC(FreeCompactClosedCategory) begin
  (A, B)::Ob
  f::Hom(A, B)
end
A, B, f = CC[:A], CC[:B], CC[:f]
dual(A)                                    # A*
dunit(A)                                   # η_A : I → A* ⊗ A
dcounit(A)                                 # ε_A : A ⊗ A* → I
mate(f)                                    # the transpose B* → A*, built from η_A and ε_B
```
tab: Lean
```lean
#check CategoryTheory.ExactPairing        -- (X Y : C): coevaluation η : 𝟙_C ⟶ X ⊗ Y, evaluation ε : Y ⊗ X ⟶ 𝟙_C, zig-zag laws
#check CategoryTheory.HasLeftDual
#check CategoryTheory.RightRigidCategory  -- every object has a right dual
#check CategoryTheory.RigidCategory       -- both duals: compact closed when symmetric
```
tab: Haskell
```haskell
-- a dual pair in a monoidal category: cup and cap satisfying the snake equations (unenforced)
data DualPair c c' = DualPair
  { cup :: () -> (c', c)       -- η_c : I → c* ⊗ c   (only meaningful in a linear/ finite setting)
  , cap :: (c, c') -> ()       -- ε_c : c ⊗ c* → I
  }
-- Hask with (,) is not compact closed; FinVect-style duals live in linear-algebra libraries
```
````
