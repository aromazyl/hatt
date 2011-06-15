Hatt
====

[Hatt] is a command-line program which prints truth tables for expressions in
classical propositional logic, and a library allowing its parser, evaluator and
truth table generator to be used in other programs.


Installation
------------

Hatt is available from [Hackage]. To install it with `cabal-install`, update
your list of known packages and then install Hatt.

    $ cabal update
    $ cabal install hatt

To build it from source, `cd` into the directory containing the Hatt source
files, including `hatt.cabal`, and run `cabal install`.


Valid Hatt expressions
----------------------

The following are all valid expression forms which can be parsed by Hatt, where
ϕ and ψ are metalinguistic variables standing in for any valid expression. The
parser isn't as smart about parentheses as it could be, so you have to follow
these rules quite literally. This shouldn't be a great hardship, but it does
mean that, for example, while `(A -> B)` is a valid expression, `A -> B` isn't.

* Variables: `P`, `Q`, `a`, `b` etc.---basically anything in the character
  class `[a-zA-Z]`
* Negation: `~ϕ`
* Conjunction: `(ϕ & ψ)`
* Disjunction: `(ϕ | ψ)`
* Conditional: `(ϕ -> ψ)`
* Biconditional: `(ϕ <-> ψ)`


Using the `hatt` command-line program
-------------------------------------

The default mode is interactive: you start the program, enter expressions at
the prompt, and their truth tables are printed. Here's an example session.

    $ hatt
    Entering interactive mode. Type `help` if you don't know what to do!
    > (A | B)
    A B | (A | B)
    -------------
    T T | T
    T F | T
    F T | T
    F F | F
    > (p -> (q & ~r))
    p q r | (p -> (q & ~r))
    -----------------------
    T T T | F
    T T F | T
    T F T | F
    T F F | F
    F T T | T
    F T F | T
    F F T | T
    F F F | T
    > (e <-> f)
    e f | (e <-> f)
    ---------------
    T T | T
    T F | F
    F T | F
    F F | T
    > exit

The `--evaluate` flag lets you pass a single expression to be evaluated
directly.

    $ hatt --evaluate="(P -> (Q | ~R))"
    P Q R | (P -> (Q | ~R))
    -----------------------
    T T T | F
    T T F | F
    T F T | F
    T F F | F
    F T T | F
    F T F | F
    F F T | T
    F F F | F

By default, `hatt` will print ASCII representations of expressions. If you have
a Unicode-capable terminal, try passing the `--pretty` option to pretty-print
expressions using the the more common logical symbols.

    $ hatt --evaluate="(P -> (Q | ~R))" --pretty
    P Q R | (P → (Q ∨ ¬R))
    ----------------------
    T T T | F
    T T F | F
    T F T | F
    T F F | F
    F T T | F
    F T F | F
    F F T | T
    F F F | F

You can enable pretty-printing while in interactive mode by using the `pretty`
command.

If you pass the `--coloured` flag, `hatt` will colour the truth values in the
tables which it prints: green for true, red for false. You can enable colouring
during interactive mode by using the `colour` command.


Using Hatt in other programs
----------------------------

Hatt exposes the `Data.Logic.Propositional` module, which provides a simple API
for parsing, evaluating, and printing truth tables.


[Hatt]:    http://extralogical.net/projects/hatt
[Hackage]: http://hackage.haskell.org/
