# ccReact: An Interacting Language for Reaction Systems

Reaction Systems (RSs) are a computational framework inspired by biochemical
systems, where entities produced by reactions can enable or inhibit other
reactions. RSs interact with the environment through a sequence of sets of
entities called the context. _ccReact_ is a novel interaction language for
implementing and verifying RSs. _ccReact_ extends the classical RS model by
allowing the specification of recursive, nondeterministic, and conditional
context sequences, thus enhancing the interactive capabilities of the models.

_ccReact_ is endowed with a rewriting logic semantics, making it executable in
the [Maude](https://maude.cs.illinois.edu/wiki/The_Maude_System) system. Our
approach enables various formal analysis techniques for RSs, including
simulation of RSs interacting with _ccReact_ processes, verification of
reachability properties, model checking of temporal (LTL and CTL) formulas, and
exploring the system evolution through
[ANIMA](https://safe-tools.dsic.upv.es/anima/), a graphical tool to better
understand its behavior.

__ccReact__ has been show to be useful for the analysis of RSs from different
domains, including computer science and biological systems. Notably, we examine
a complex breast cancer case study [[3]](#references), demonstrating that our
analysis can suggest improvements to the administration of monoclonal antibody
therapeutic treatments in certain scenarios.

Details about _ccReact_ can be found in this [paper](./paper.pdf) and the
references [1,2].


## Getting started

The project was tested in [Maude 3.2.1](http://maude.cs.illinois.edu/) and with
[umaudemc](https://github.com/fadoss/umaudemc) for model checking LTL and CTL
temporal formulas. 

## Files

- [`syntax`](./syntax.maude): Basic sorts and operators to define reactions,
  contexts and _ccReact_ processes. 
- [`semantics`](./semantics.maude): Rules defining the evaluation of contexts
  and how the system evolve.

The directly [`case-studies`](./case-studies/) contains some simple examples to
illustrate the definition of RSs and the specification of properties. This
directory also contains different analyses concerning the protein signaling
networks in the HER2-positive subtype breast cancer [[3]](#references) and some
benchmarks of CTL formulas from [[4]](#references).

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
