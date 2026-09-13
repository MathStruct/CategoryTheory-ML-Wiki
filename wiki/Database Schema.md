#definition #example

A **database** is a system of interlocking tables; each table has an **ID column** of unique row labels, and the other columns are references: **foreign keys** (internal references to rows of another table, e.g. `WorksIn`, `Mngr`, `Secr`) or external references to strings/integers (`FName`, `DName`). Foreign-key labels can be renamed consistently ($1 \mapsto 1001$) without changing the meaning; external labels cannot (Ruth $\neq$ Bruce).

A **database schema** is the reference structure drawn as a "Hasse diagram for a database": one (black) vertex per table, one (white) vertex per external type, one arrow per non-ID column pointing in the direction of reference ([[7S Chapter 3 Exercises#Exercise 3.3|7S Exercise 3.3]]: as many arrows as non-ID columns), together with **business rules** — path equations. That is, a schema is a [[Presentation of a Category]]; the data is a [[C-Set]] $\mathcal{C} \to \mathbf{Set}$.

> Sources: 7 Sketches §3.1 (Eqs. 3.1–3.5), Remark 3.20, §3.4, §3.6; Kittenlab Lecture 6 ("you can think of $\mathsf{C} = \mathrm{Path}(G)$ as a database schema"; graphs as two-table databases); FQL, the functorial query language.

**Example (mySchema).** Tables `Employee` (FName, WorksIn, Mngr) and `Department` (DName, Secr), with rules

$$
\mathsf{Department.Secr.WorksIn} = \mathsf{Department}, \qquad \mathsf{Employee.Mngr.WorksIn} = \mathsf{Employee.WorksIn}
$$

("every department's secretary works in that department; every employee's manager works in the employee's department"): `easySchema` + constraints = `mySchema`.

```tikz
\usepackage{tikz-cd}
\begin{document}
\begin{tikzcd}
\mathsf{Employee} \arrow[loop left, "\mathsf{Mngr}"] \arrow[r, "\mathsf{WorksIn}", shift left] \arrow[d, "\mathsf{FName}"'] & \mathsf{Department} \arrow[l, "\mathsf{Secr}", shift left] \arrow[d, "\mathsf{DName}"] \\
\mathsf{string} & \mathsf{string}
\end{tikzcd}
\end{document}
```

**Data integration** accounted for 40% of IT budgets in 2008 and over half of migration projects fail; category theory lets one *prove up front* that a [[Data Migration Functor|migration]] along a functor $F : \mathcal{A} \to \mathcal{B}$ between schemas (e.g. Economy/First-Class seats $\mapsto$ Airline Seat, Eq. 3.5) yields data satisfying the target's constraints. The four ingredients: schemas are categories, instances are functors to $\mathbf{Set}$, schema mappings are functors, migration is by [[Adjunction|adjoints]]. Schemas are "ad hoc — formed for a particular purpose — and there is nothing wrong with that" (§2.2.3). Further reading: [Spi12; SW15b; Sch+17] (algebraic databases with an attached programming language).

````tabs
tab: Julia
```julia
using Catlab
@present SchMySchema(FreeSchema) begin
  (Employee, Department)::Ob
  Mngr::Hom(Employee, Employee)
  WorksIn::Hom(Employee, Department)
  Secr::Hom(Department, Employee)
  Str::AttrType
  FName::Attr(Employee, Str); DName::Attr(Department, Str)
  compose(Secr, WorksIn) == id(Department)
  compose(Mngr, WorksIn) == WorksIn
end
@acset_type MySchema(SchMySchema)
db = @acset MySchema{String} begin
  Employee = 3; Department = 2
  FName = ["Alan", "Ruth", "Kris"]; WorksIn = [1, 1, 2]; Mngr = [2, 2, 3]
  DName = ["Sales", "IT"]; Secr = [1, 3]
end
db[:Secr], db[:WorksIn]      # ([1, 3], [1, 1, 2]): Secr ⋅ WorksIn == id holds
```
tab: Haskell
```haskell
-- a schema as data: tables, foreign keys, attributes, and path equations
data Schema = Schema
  { tables     :: [String]
  , foreignKey :: [(String, String, String)]   -- (column, from table, to table)
  , attribute  :: [(String, String, String)]   -- (column, table, external type)
  , rules      :: [([String], [String])]       -- equal paths of column names
  }
mySchema :: Schema
mySchema = Schema ["Employee", "Department"]
  [("Mngr","Employee","Employee"), ("WorksIn","Employee","Department"), ("Secr","Department","Employee")]
  [("FName","Employee","string"), ("DName","Department","string")]
  [(["Secr","WorksIn"], []), (["Mngr","WorksIn"], ["WorksIn"])]
```
````
