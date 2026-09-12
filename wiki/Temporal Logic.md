#example #annotation

**Safety proofs in temporal logic** (7 Sketches §7.5.3). In the [[Topos of Behavior Types]] $\mathbf{BT}$, the [[Internal Language of a Topos|internal language]] becomes a temporal logic: variables are behavior types (the altimeter reading $\theta \in \mathbb{R}_{\geq 0}$ changing continuously, the thruster position in $[0,1]$), and *behavior contracts* are [[Predicate|predicates]] on them. For instance "the pilot engages the thruster within one second of the dials showing $\mathsf{bad\_pos}$, and keeps it engaged for five seconds" is the predicate $p : \mathsf{dials} \times \mathsf{thrusters} \to \Omega$,
$$\forall(t : \mathbb{R}).\ @_t\, \mathsf{bad\_pos}(D) \Rightarrow \exists(r : \mathbb{R}).\ (0 < r < 1) \wedge \forall(r' : \mathbb{R}).\ 0 \leq r' \leq 5 \Rightarrow @_{t + r + r'}\, \mathsf{engaged}(T). \tag{7.82}$$
Here $@_t$ is a [[Modality]] (of type (c) in Proposition 7.71): $@_t(q)$ says "$q$ holds in some small enough neighbourhood of $t$".

> Sources: 7 Sketches §7.1, §7.5.3, Eq. (7.82), §7.6; [SS18] (Schultz–Spivak, *Temporal Type Theory*), [SSV18].

- Given actual sections $D \in \mathsf{dials}(U)$, $T \in \mathsf{thrusters}(U)$, the predicate holds on some open part of $U$ — its truth value in $\Omega(U)$. If the pilot always upholds the contract the value is all of $U$; contract breaches are recorded as the complement.
- Axioms like (7.82) for each component (pilot, airplane, radars) are combined by the logic along the [[Wiring Diagram]] of the system: "if the pilot and the airplane and at least one of the three radars uphold their contracts then safe separation is maintained". Compositionality means the proof for a subsystem can be reused when it is wired into a larger one.
- The origin of this material is a project between MIT, Honeywell and NASA on the National Airspace System [SSV18].

````tabs
tab: Haskell
```haskell
-- a toy discrete-time version of contract (7.82), sampled at integer times:
-- whenever badPos holds at t, engaged holds at t+1, …, t+6 (r = 1, r' = 0..5)
contract :: [Bool] -> [Bool] -> Bool
contract badPos engaged = and [ holds (t + 1) | (t, True) <- zip [0 ..] badPos ]
  where holds t0 = and [ engaged !! t | t <- [t0 .. t0 + 5], t < length engaged ]
```
````
