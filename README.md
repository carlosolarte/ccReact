# ccReact: An Interacting Language for Reaction Systems

Reaction Systems (RSs) are a computational framework inspired by biochemical
systems, where entities produced by reactions can enable or inhibit other
reactions. RSs interact with the environment through a sequence of sets of
entities called the context. __ccReact__ is a novel interaction language for
implementing and verifying RSs. _ccReact_ extends the classical RS model by
allowing the specification of recursive, nondeterministic, and conditional
context sequences, thus enhancing the interactive capabilities of the models.

__ccReact__ is endowed with a rewriting logic semantics, making it executable in
the [Maude](https://maude.cs.illinois.edu/wiki/The_Maude_System) system. Our
approach enables various formal analysis techniques for RSs, including
simulation of RSs interacting with __ccReact__ processes, verification of
reachability properties, model checking of temporal (LTL and CTL) formulas, and
exploring the system evolution through
[ANIMA](https://safe-tools.dsic.upv.es/anima/), a graphical tool, to better
understand its behavior.

__ccReact__ has been show to be useful for the analysis of RSs from different
domains, including computer science and biological systems. Notably, we examine
a complex breast cancer case study [[3]](#references), demonstrating that our
analysis can suggest improvements to the administration of monoclonal antibody
therapeutic treatments in certain scenarios.

Details about __ccReact__ can be found in this [paper](./paper.pdf) and the
references [[1,2](#references)].


## Getting started

The project was tested in [Maude 3.5](http://maude.cs.illinois.edu/) and with
[umaudemc](https://github.com/fadoss/umaudemc) for model checking LTL and CTL
temporal formulas. 

### Files

- [`syntax`](./syntax.maude): Basic sorts and operators to define reactions,
  contexts and __ccReact__ processes. 
- [`semantics`](./semantics.maude): Rules defining the evaluation of contexts
  and how the system evolve.

The directly [`case-studies`](./case-studies/) contains some simple examples to
illustrate the definition of RSs and the specification of properties. This
directory also contains different analyses concerning the protein signaling
networks in the HER2-positive subtype breast cancer [[3]](#references) and some
benchmarks of CTL formulas from [[4]](#references).

### Analysis Methods

Let us illustrate how to use the system. Consider the simple
[`example1`](./case-studies/example1.maude). In this file, we first include the
needed files: 

```
load ../semantics .
```

and then create a module defining the reaction system:

```
mod System is
 inc SEMANTICS .
 
 ...

endm
```

The module `System` defined the entities of the RS:

```
 ops a b c d : -> Entity [ctor] .
```

The set of reactions (in this simple case, there is only one reaction):

```
 eq reactions = [ (a,b) ; (c) ; (b) ] .
```

and a process providing the context to the reaction system: 

```
 op proc : -> Process .
 eq proc = { (a , b) } . { a } . { c } . { c } . 0 .
```

Now that the reaction system is specified, we can use Maude to simulate the
behavior of the system, perform reachability analysis and model check temporal
formulas. 

We first load the reaction system into Maude:

```
$> maude example1.maude
                     \||||||||||||||||||/
                   --- Welcome to Maude ---
                     /||||||||||||||||||\
             Maude 3.5 built: Sep 25 2024 12:00:00
             Copyright 1997-2024 SRI International
                   Fri Nov  1 13:00:36 2024
Maude>
```

The command `rew` can be used to perform rewriting and then, observe the
behavior of the system. For instance, the following commands show how to
perform one and two steps of computation in the rewriting system:

```
Maude> rew [1] init(proc) .
rewrite [1] in System : init(proc) .
rewrites: 27 in 0ms cpu (0ms real) (~ rewrites/second)
result State: {proc: {a}. {c}. {c}. 0 ; in: empty ; out: b ; ctx: a, b}

Maude> rew [2] init(proc) .
rewrite [2] in System : init(proc) .
rewrites: 52 in 0ms cpu (0ms real) (~ rewrites/second)
result State: {proc: {c}. {c}. 0 ; in: b ; out: b ; ctx: a}
```

More interesting, we can search for all the possible reachable states from the
initial state: 

```
Maude> search init(proc) =>* S:State .
search in System : init(proc) =>* S:State .

Solution 1 (state 0)
states: 1  rewrites: 2 in 0ms cpu (0ms real) (~ rewrites/second)
S:State --> {proc: {(a, b)}. {a}. {c}. {c}. 0 ; in: empty ; out: empty ; ctx: empty}

Solution 2 (state 1)
states: 2  rewrites: 27 in 0ms cpu (0ms real) (~ rewrites/second)
S:State --> {proc: {a}. {c}. {c}. 0 ; in: empty ; out: b ; ctx: a, b}

Solution 3 (state 2)
states: 3  rewrites: 52 in 0ms cpu (0ms real) (~ rewrites/second)
S:State --> {proc: {c}. {c}. 0 ; in: b ; out: b ; ctx: a}

Solution 4 (state 3)
states: 4  rewrites: 64 in 0ms cpu (0ms real) (~ rewrites/second)
S:State --> {proc: {c}. 0 ; in: b ; out: empty ; ctx: c}

Solution 5 (state 4)
states: 5  rewrites: 76 in 0ms cpu (0ms real) (~ rewrites/second)
S:State --> {proc: 0 ; in: empty ; out: empty ; ctx: c}

No more solutions.
states: 5  rewrites: 76 in 0ms cpu (0ms real) (~ rewrites/second)
```

Using [umaudemc](https://github.com/fadoss/umaudemc), it is also possible 
to model check temporal properties of the system:

```
$> umaudemc check example1 "init(proc)" "<> b"
The property is satisfied in the initial state (2 system states, 39 rewrites, 1 Büchi state)
```

This command is showing that _eventually_, the entity `b` is produced. For more
compelling examples of LTL and CTL formulas, see the end of the files of the
breast cancer case study (e.g., [BT474.maude](./case-studies/BT474.maude). 

## References

[1] Demis Ballis, Linda Brodo, Moreno Falaschi and  Carlos Olarte: _Process
calculi and rewriting techniques for analyzing reaction systems_. In:
Computational Methods in Systems Biology (CMSB’24), Springer Nature
Switzerland. Pisa, 2024. 

[2]  Demis Ballis, Linda Brodo, Moreno Falaschi and  Carlos Olarte: _ccReact: a
Rewriting Framework for the Formal Analysis of Reaction Systems_. Submitted to
__International Journal on Software Tools for Technology Transfer__ (2024). The
PDF of the submitted version is available [here](./paper.pdf).

[3] Silvia von der Heyde, Christian Bender, Frauke Henjes, Johanna Sonntag,
Ulrike Korf, Tim Beißbarth: Boolean ErbB network reconstructions and
perturbation simulations reveal individual drug response in different breast
cancer cell lines. BMC Syst Biol. 2014

[4] Artur Meski, Wojciech Penczek, Grzegorz Rozenberg: Model checking temporal
properties of reaction systems. Inf. Sci. 313: 22-42 (2015)
