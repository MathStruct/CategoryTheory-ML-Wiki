#definition #example #annotation

An **olog** (ontology log; Spivak–Kent) is a [[Database Schema]] written so that a person can read it: a [[Graph]] whose boxes are **types**, whose arrows are **aspects**, together with **facts** — declared equalities of paths. Every box is labelled by an indefinite noun phrase ("a person", "a pair $(w, m)$ where $w$ is a woman and $m$ is a man"), every arrow by a verb phrase such that *box – arrow – box* reads as an English sentence ("a person *has as mother* a woman"), and every arrow must be a [[Function]]. Mathematically an olog is a [[Presentation of a Category|category presentation]]; its *instances* — sets of examples for every box and functions for every arrow satisfying the facts — are [[C-Set|functors to $\mathbf{Set}$]]. Ologs are Category Theory for Scientists' bridge "between mathematics and various conceptual landscapes".

> Sources: CTfS §2.3 (Rules 2.3.1.2, 2.3.2.8, Warning 2.3.2.2, Remarks 2.3.2.3–2.3.2.5, Examples 2.3.3.2, 2.3.3.5, §2.3.3.4, §2.3.3.8), §2.4.1.17, §2.5.1.7, Example 2.7.5.7, §3.1.2.8, §3.5.2.14, Example 4.1.1.19, Example 5.3.3.9; [SK] Spivak & Kent, *Ologs: A Categorical Framework for Knowledge Representation* (PLoS ONE 2012).

## Types, aspects, facts

- **Types** (CTfS §2.3.1). A box names a *type* of thing — "a man", "an automobile", "a pair $(a, w)$ where $w$ is a woman and $a$ is a blue automobile owned by $w$" — i.e. a set of instances, each called by the box's label. Rules of good practice: the label begins with "a" or "an"; refers to a distinction made and recognizable by the author; refers to a distinction for which instances can be documented; declares all variables of compound structures.
- **Aspects** (CTfS §2.3.2). "An aspect of a thing $x$ is a way of viewing it, a particular way in which $x$ can be regarded or measured": "a molecule *has as molecular mass (Da)* a positive real number". The arrow must be *functional*: every source instance gives exactly one target instance. "A person *has* a child" and "a mechanical pencil *uses* a piece of lead" are **invalid** aspects (zero or many children, several leads). They are repaired by reversing the arrow ("a child *has as father* a father"), by changing the meaning ("a person *has as inner child* a child"), or by introducing a type of pairs with two aspects — a [[Span]]:

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}[column sep=small, cells={nodes={draw=black, rounded corners, align=center}}]
 & \parbox{4.4cm}{\centering a pair $(p, c)$ where $p$ is a person, $c$ is a car, and $p$ owns $c$} \arrow[dl, "p"'] \arrow[dr, "c"] & \\
\text{a person} & & \text{a car}
\end{tikzcd}
\end{document}
```

- **Facts** (CTfS §2.3.3). A fact declares two paths with the same source and target equivalent — a commutative diagram, marked with a check ✓ or written as an equation such as "$A \xrightarrow{f} B \xrightarrow{g} D \simeq A \xrightarrow{h} C \xrightarrow{i} D$". "It is the notion of path equivalences that make category theory so powerful."

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}[column sep=large, cells={nodes={draw=black, rounded corners, align=center}}]
\text{a person} \arrow[r, "\text{has as parents}"] \arrow[dr, "\text{has as mother}"'] & \parbox{4cm}{\centering a pair $(w, m)$ where $w$ is a woman and $m$ is a man} \arrow[d, "\text{yields as } w"] \\
 & \text{a woman}
\end{tikzcd}
\end{document}
```

"A woman parent is equivalent to a mother" (CTfS Olog 2.18). A **non**-commuting diagram, e.g. "a person *has as father* a man *lives in* a city" vs. "a person *lives in* a city", does not say that nobody lives where their father lives — only that it is not *always* so (CTfS Example 2.3.3.2).

## Reading ologs as English

- **Paths**: insert "which" (or "who") between arrows: "a child is a person, who has as parents a pair $(w, m)$ …, which yields, via the value of $w$, a woman" (CTfS §2.3.2.4).
- **Facts** (CTfS Formula 2.21): "Given $x$, a person, consider the following. We know that $x$ is a person, which has an address, which is in a city, that we'll call $P(x)$. We also know that $x$ is a person, which lives in a city, that we'll call $Q(x)$. Fact: whenever $x$ is a person, we will have $P(x) = Q(x)$."
- **Universal constructions have canonical labels**: a [[Product]] $c \times d$ is "a pair $(x, y)$ where $x$ is $c$ and $y$ is $d$" with projections "yields, as $x$"; the induced arrow reads "yields, insofar as it $p$ $c$ and $q$ $d$," (CTfS §2.4.1.17); a [[Coproduct]] is "a $c$ or a $d$" — keeping ducks twice if they both fly and swim; an [[Pullback|image or fiber product]] *defines* new types: "a mother" is the image of "has as mother", and "a customer that is wealthy and loyal" is the pullback of the wealthy and the loyal customers (CTfS §2.3.3.8, §2.5.1.7). Pulling back along an injection gives "a rib which is made by a cow" (CTfS Example 2.7.5.7). A [[Subobject Classifier|truth value]] classifies subtypes.

## Examples from Category Theory for Scientists

- **Biology**: "a DNA sequence *is transcribed to* an RNA sequence *is translated to* a protein" = "a DNA sequence *codes for* a protein" (✓). Arginine *is* an amino acid *found in dairy*, *has* an electrically-charged side chain, … (CTfS Olog 2.8).
- **Telephones** (CTfS Exercise 2.3.3.6): "an operational landline phone *is* a physical phone *is currently located in* a region" = "… *is assigned* a phone number *has* an area code *corresponds to* a region" — a fact that fails for mobile phones.
- **Playing cards** (CTfS Example 3.4.1.3): an olog in which every arrow reads "is" is a [[Preorder]]: "a 4 of diamonds is a diamond, is a red card, is a card". See [[Hasse Diagram]].
- **Monoid actions** (CTfS §3.1.2.8): an olog with one box is a [[Monoid Action]]: "a character position *when moved up results in* a character position", with facts "up then down = do nothing" and "up then right = right then up".
- **A category, as an olog** (CTfS Example 4.1.1.19): "a pair $(g, f)$ of composable morphisms *has as composition* a morphism in $\mathcal{C}$", with "has as domain", "has as codomain".
- **The scientific method** (CTfS cover and Example 5.3.3.9): "a hypothesis analyzed by a person produces a prediction, which motivates the specification of an experiment, which when executed results in an observation, which analyzed by a person yields a hypothesis" is *not* a valid olog — different people derive different hypotheses — until the hidden context "which scientist, with which model" is made explicit by a [[Monad]].

## World-views and granularity

"The author of an olog has a world-view, some fragment of which is captured in the olog." Person B's "a marriage *includes* a man / *includes* a woman" may look wrong to person A, but such disputes are "discrepancies between world-views", not structural errors (CTfS Warning 2.3.2.2). Rules exist "to ensure that an olog is structurally sound, rather than that it correctly reflects reality". "An object *has* a weight" was once valid and is not since we know about the moon; "to build a model we need to choose a level of granularity and try to stay within it, or the whole model evaporates into the nothingness of truth!" Shared world-views of a community can be assembled into a [[Sheaf]] of ologs (CTfS §5.2.3.11).

## Ologs as databases

Because every aspect is a function, an olog is a [[Database Schema]] and can be *filled with data*: "a moon *orbits* a planet" is a table with rows The Moon ↦ Earth, Phobos ↦ Mars, Deimos ↦ Mars, Ganymede ↦ Jupiter, Titan ↦ Saturn (CTfS Example 3.5.2.15). "A database schema is nothing but an olog in disguise; the difference is basically the readability requirements." Instances, [[Data Migration Functor|migration]] along olog morphisms, and the [[Category of Elements|RDF triple view]] all apply.

````tabs
tab: Julia
**Docs:** [ACSets API](https://algebraicjulia.github.io/ACSets.jl/stable/api/) · [Theories & presentations](https://algebraicjulia.github.io/Catlab.jl/v0.16/apis/theories/) · [Vignette: category of elements](https://algebraicjulia.github.io/Catlab.jl/v0.16/generated/sketches/cat_elements/)
```julia
using Catlab
# An olog is a schema whose boxes and arrows read as English; CTfS Olog (2.18):
# "a person has as parents a pair (w, m); the woman parent is the mother".
@present SchFamily(FreeSchema) begin
  (Person, Parents, Woman)::Ob
  parents::Hom(Person, Parents)   # "has as parents"
  w::Hom(Parents, Woman)          # "yields as w"
  mother::Hom(Person, Woman)      # "has as mother"
  parents ⋅ w == mother           # the fact (✓): a mother is a woman parent
end
@acset_type Family(SchFamily)
F = @acset Family begin
  Person = 3; Parents = 2; Woman = 2
  parents = [1, 1, 2]; w = [1, 2]; mother = [1, 1, 2]
end
# instances are not checked against facts automatically; check the fact by hand:
all(F[F[p, :parents], :w] == F[p, :mother] for p in parts(F, :Person))   # true
```
tab: Lean
```lean
-- An olog with a fact, as a structure: types are `Type`s, aspects are functions,
-- and the fact is a proof obligation that every instance must discharge.
structure FamilyOlog where
  Person : Type
  Parents : Type
  Woman : Type
  hasAsParents : Person → Parents
  yieldsAsW : Parents → Woman
  hasAsMother : Person → Woman
  fact : ∀ x, yieldsAsW (hasAsParents x) = hasAsMother x   -- "a woman parent is a mother"
```
tab: Haskell
```haskell
-- boxes are types, aspects are (total!) functions, facts are equations we promise to keep
data Person  = Person { name :: String, parents :: Parents }
data Parents = Parents { w :: Woman, m :: Man }
newtype Woman = Woman String deriving (Eq, Show)
newtype Man   = Man String deriving (Eq, Show)

hasAsMother :: Person -> Woman
hasAsMother = w . parents          -- the fact  (has as parents) ; (yields as w) = (has as mother)
                                   -- holds by definition here
```
````
