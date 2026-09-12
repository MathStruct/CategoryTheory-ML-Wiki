#exercise #solution #proof

**Exercise 4.4.5.** Show that functoriality of the sum preserves identity: $\langle \mathrm{id}_a, \mathrm{id}_b \rangle = \mathrm{id}_{a+b}$ ([[Sum Type]]).

## Solution

$\langle \mathrm{id}, \mathrm{id} \rangle = [\mathsf{Left} \circ \mathrm{id}, \mathsf{Right} \circ \mathrm{id}] = [\mathsf{Left}, \mathsf{Right}]$, and $\mathrm{id}_{a+b}$ also satisfies $\mathrm{id} \circ \mathsf{Left} = \mathsf{Left}$, $\mathrm{id} \circ \mathsf{Right} = \mathsf{Right}$; uniqueness of copairing gives equality. (Same as [[7S Exercise 6.17]] (4).)

> Sources: DaoFP Exercise 4.4.5.
