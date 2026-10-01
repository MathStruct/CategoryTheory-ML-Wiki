#definition #theorem #example #program

**Compiler correctness** in its algebraic form (Morris 1973; Thatcher, Wagner & Wright 1981) is a commuting square. The source language is the [[Initial Algebra]] of its signature, its semantics is the unique homomorphism into an algebra of meanings, and the **compiler is also a homomorphism** — into an algebra of target code. Correctness says the two routes from source programs to meanings agree:

```tikz
\usepackage{tikz-cd}
\newcommand{\llbracket}{[\![}
\newcommand{\rrbracket}{]\!]}
\begin{document}
\begin{tikzcd}[column sep=huge, row sep=large]
\text{Source} \arrow[r, "\mathrm{compile}"] \arrow[d, "\llbracket - \rrbracket_S"'] & \text{Target} \arrow[d, "\llbracket - \rrbracket_T"] \\
\text{Meanings}_S \arrow[r, "\mathrm{encode}"'] & \text{Meanings}_T
\end{tikzcd}
\end{document}
```

Because all four arrows are homomorphisms out of (or between) algebras for the same signature, and the source is *initial*, it suffices to check the square on the generators: the compiler's clause for each constructor must be matched by an operation on target meanings. The proof is then structural induction, often after **strengthening** the statement (for a stack machine: running the compiled code on *any* stack pushes the value) so that the induction hypothesis is strong enough.

> Sources: Morris, *Advice on structuring compilers and proving them correct*, POPL 1973; Thatcher, Wagner & Wright, *More on advice on structuring compilers and proving them correct*, Theor. Comput. Sci. 15 (1981); Bahr & Hutton, *Calculating correct compilers*, J. Funct. Programming 25 (2015); Leroy, *Formal verification of a realistic compiler*, Commun. ACM 52(7) (2009) (CompCert: simulation diagrams per pass); Pnueli, Siegel & Singerman, *Translation validation*, TACAS 1998, and Necula, *Translation validation for an optimizing compiler*, PLDI 2000; Lopes, Lee, Hur, Liu & Regehr, *Alive2: bounded translation validation for LLVM*, PLDI 2021; Elliott, *Compiling to categories*, ICFP 2017 ([[Curry-Howard-Lambek Correspondence]]). Background: [[Initial Algebra]], [[Functorial Semantics]].

## From squares to simulations

For a realistic compiler with many passes and an operational target, the square becomes a **simulation**: every step (or sequence of steps) of the source is matched by target steps that preserve a relation between states. CompCert proves one simulation per pass and composes them — simulations compose like [[Logical Relations|logical relations]], and forward simulation plus determinism of the target gives backward simulation, so correctness of the whole pipeline is the composite of the squares. For languages with linking, the statement must also say what happens when compiled code meets code compiled by someone else — *compositional* compiler correctness, which needs a cross-language relation at the boundary.

## Proving once versus checking every time

| strategy | what is proved | trusted at compile time |
|---|---|---|
| verified compiler (CompCert, CakeML) | the compiler is correct for *all* inputs | nothing beyond the proof checker |
| **translation validation** (Alive2) | *this* output is correct for *this* input | the validator |
| testing / fuzzing | agreement on sampled inputs | the test oracle |

Translation validation checks the square instance by instance, after the fact; it is how LLVM peephole optimisations are checked in practice, and it is the only realistic option for a compiler one does not control.

## Compiler passes as functors

Seen from further away, a compiler pass is a functor between categories of programs (objects: types or interfaces; morphisms: programs), and the requirement that compiling a composite equals composing the compiled parts is **functoriality** — the property that makes separate compilation work. Morris' square is the naturality square of the semantics with respect to that functor.

## Sophia

Sophia stores lowerings as `LOWERS_TO` edges and wants evidence for them: a per-instance translation-validation report (the Alive2 strategy) for `Term → Op → Instr`, because proving MLIR and LLVM correct is out of reach, and functoriality tests for frontends ("elaborate(f ∘ g) = elaborate(f) ∘ elaborate(g)"). See [Equivalence and Witnesses](https://mathstruct.org/Sophia/vault/Design/Equivalence-and-Witnesses), [Multi-AST Layering](https://mathstruct.org/Sophia/vault/Design/Multi-AST-Layering) and [Trusted Computing Base](https://mathstruct.org/Sophia/vault/Design/Trusted-Computing-Base).

````tabs
tab: Julia
**Docs:** plain Julia — Catlab has no dedicated API for this; related: [Catlab v0.16 docs](https://algebraicjulia.github.io/Catlab.jl/v0.16/) · [GATlab standard library](https://algebraicjulia.github.io/GATlab.jl/stable/stdlib/)
```julia
using Random; Random.seed!(7)
# Source: arithmetic expressions, the initial algebra of the signature {lit n, add, mul}.
# Source semantics: the fold into the algebra (ℤ, +, ×).
eval_(e) = e[1] == :lit ? e[2] : e[1] == :add ? eval_(e[2]) + eval_(e[3]) : eval_(e[2]) * eval_(e[3])
# Target: stack-machine code. The compiler is ALSO a fold — into the algebra of code sequences:
#   lit n ↦ [PUSH n],  add(c₁, c₂) ↦ c₁ ++ c₂ ++ [ADD],  mul(c₁, c₂) ↦ c₁ ++ c₂ ++ [MUL]
comp(e) = e[1] == :lit ? [(:push, e[2])] : [comp(e[2]); comp(e[3]); [(e[1] == :add ? :ADD : :MUL, 0)]]
function exec(code, stack = Int[])
    for (op, n) in code
        op == :push ? push!(stack, n) : (b = pop!(stack); a = pop!(stack); push!(stack, op == :ADD ? a + b : a * b))
    end
    stack
end
# Morris' square: compile, then run  ==  evaluate, then encode (push the value).
randexpr(d) = d == 0 || rand() < 0.3 ? (:lit, rand(-5:5)) : (rand([:add, :mul]), randexpr(d - 1), randexpr(d - 1))
es = [randexpr(5) for _ in 1:200]
all(exec(comp(e)) == [eval_(e)] for e in es)               # true: the square commutes on 200 random terms
# The proof is by induction because both sides are homomorphisms out of the initial algebra; the
# strengthened statement that makes the induction go through: running comp(e) on ANY stack pushes eval(e).
all(exec(comp(e), [7, 8]) == [7, 8, eval_(e)] for e in es)  # true
# comp is a homomorphism: compiling a composite is composing the compiled parts
e1, e2 = es[1], es[2]
comp((:add, e1, e2)) == [comp(e1); comp(e2); [(:ADD, 0)]]   # true
```
tab: Lean
```lean
import Mathlib
inductive Expr where
  | lit : Int → Expr
  | add : Expr → Expr → Expr

def Expr.eval : Expr → Int
  | .lit n => n
  | .add a b => a.eval + b.eval

inductive Instr where
  | push : Int → Instr
  | add : Instr

-- the compiler is a fold into the algebra of code sequences
def Expr.comp : Expr → List Instr
  | .lit n => [.push n]
  | .add a b => a.comp ++ b.comp ++ [.add]

def exec : List Instr → List Int → List Int
  | [], s => s
  | .push n :: c, s => exec c (n :: s)
  | .add :: c, b :: a :: s => exec c ((a + b) :: s)
  | .add :: c, s => exec c s

-- Morris' square, in the strengthened form that makes the induction work:
-- running comp e followed by any code c on any stack s = running c on (eval e :: s).
theorem comp_correct (e : Expr) : ∀ (c : List Instr) (s : List Int),
    exec (e.comp ++ c) s = exec c (e.eval :: s) := by
  induction e with
  | lit n => intro c s; rfl
  | add a b iha ihb =>
    intro c s
    simp only [Expr.comp, Expr.eval, List.append_assoc, List.singleton_append]
    rw [iha, ihb]
    rfl

theorem comp_correct' (e : Expr) : exec e.comp [] = [e.eval] := by
  simpa [exec] using comp_correct e [] []
```
````
