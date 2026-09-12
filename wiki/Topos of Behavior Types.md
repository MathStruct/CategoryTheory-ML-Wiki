#definition #example #annotation

The **topos of behavior types** is $\mathbf{BT} := \mathbf{Shv}(\mathbb{I}\mathbb{R}, \mathrm{Op})$, the [[Topos]] of [[Sheaf|sheaves]] on the [[Interval Domain]]. A sheaf $S \in \mathbf{BT}$ is a *behavior type*: a section $s \in S(U)$ is "an event that takes place throughout the interval $U$"; restriction $s|_V$ views it over a sub-interval; the sheaf condition says matching local events glue uniquely to a global one. Morphisms are natural transformations. Roughly: behavior types are "sets whose elements can change through time".

> Sources: 7 Sketches §7.1 ("How can we prove our machine is safe?"), Eqs. (7.1)–(7.2), §7.5 ("A topos of behavior types"), Examples 7.78, 7.79, 7.81, Exercise 7.80, §7.5.3, [SS18], [SSV18].

## Examples of behavior types

- **Constant sheaf** $\underline{A}$ (Example 7.78): $\underline{A}(U) = A$ — the behavior "always $7$".
- **Local functions** (Example 7.79): $F_X(U) = \{f : U \to X \text{ continuous}\}$, and $G_X(U) = \{f : U \cap \mathbb{R} \to X \text{ continuous}\}$ for a space $X$; e.g. the distance $S$ of a vehicle over $[a, b]$ is a continuous $[a,b] \to \mathbb{R}$, an action signal $A$ is a piecewise constant function into $\{\mathsf{go}, \mathsf{stop}\}$.
- **Hybrid dynamical systems** (Example 7.81): continuous flow along a vector field interrupted by discrete jumps; each converts into a sheaf on $\mathbb{I}\mathbb{R}$.
- **Sheaf of people**, the running example of §7.4: $S(U)$ = people alive throughout $U$.
- The [[Subobject Classifier]]: $\Omega(U)$ = open subsets of $U$, so a proposition such as "Bob likes the weather" returns the open set of times at which it holds.

## Systems as wiring diagrams

A system of interacting components (sensor, controller, motor of Eq. 7.1) is a [[Wiring Diagram]] whose wires are behavior types and whose boxes are **behavior contracts**: [[Predicate|predicates]] on the product of the port types, e.g. the controller maintains $\{(S', A) \mid (A = \mathsf{go}) \Rightarrow \ddot{S} > 1 \wedge (A = \mathsf{stop}) \Rightarrow \ddot{S} = 0\}$ (Eq. 7.2). Because everything is compositional ("compatible with opening up lower-level subsystems"), properties of the whole follow from properties of the components plus the wiring pattern — this is the [[Temporal Logic]] safety-proof programme of [SS18].

````tabs
tab: Julia
```julia
# a behavior type sampled on basic opens: sections over o_[a,b] are functions on (a,b)
struct Behavior; sections::Function; end         # sections(a, b) :: Vector of allowed functions
# constant behavior type on A = {go, stop}: a section over any interval is a constant
const_A = Behavior((a, b) -> [t -> :go, t -> :stop])
# piecewise-constant signals, restriction = restriction of the function
restrict(s, a, b) = t -> (a < t < b ? s(t) : nothing)
```
tab: Lean
```lean
import Mathlib
#check @TopCat.Sheaf              -- BT would be `TopCat.Sheaf (Type) (TopCat.of IntervalDomain)`
#check @TopCat.Presheaf.IsSheaf
```
tab: Haskell
```haskell
-- a behavior type as "sections over an interval", with restriction
newtype Behavior a = Behavior { over :: (Double, Double) -> [Double -> a] }
constant :: [a] -> Behavior a
constant as = Behavior (\_ -> map const as)
```
````
