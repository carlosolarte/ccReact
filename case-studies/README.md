# Case Studies

## Simple Examples 
The following files present very simple examples to understand the syntax of
__ccReact__ and the Maude's commands that can be used to verify temporal properties
of the model:

- `example1.maude` 
- `example2.maude` 
- `example3.maude` 

## Breast cancer case study 
The case studies considering the three different kinds of breast cancer,
identified as BT474, HCC1954, and SKBR3, can be found in the files with the
same name for the short-term experiments. For the long-term experiments, the
suffix `-ext` is added to the name of the file. 

In the end of each of each of the following Maude's file, different queries
using the command `search` and `umaudemc` for LTL/CTL model checking can be
found. Some of these queries are explained in the [paper](../paper.pdf):

- `BT474-ext.maude`
- `BT474.maude`
- `HCC1954-ext.maude`
- `HCC1954.maude`
- `SKBR3-ext.maude`
- `SKBR3.maude`

## Other examples 

From [1], we consider the following case studies:
- `bcounter`:  a n-bit cyclic binary counter
- `mutex`: mutual exclusion protocol
- `hshock`: eukaryotic heat shock response system
- `pipeline`: abstract pipeline system

## References

- [1] Artur Meski, Wojciech Penczek, Grzegorz Rozenberg: Model checking
  temporal properties of reaction systems. Inf. Sci. 313: 22-42 (2015)

