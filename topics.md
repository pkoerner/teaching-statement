# Theses Topics


## Based on my Research

### LTSmin

LTSmin is a language-agnostic model checker written in C.
During my master thesis, I worked on a frontend for the B language.
I hooked up ProB with LTSmin using ZMQ (roughly) in order to expose ProB's next-state and labelling function.
This allows using LTSmin's state-of-the-art model checking algorithm implementations (parallel LTL, guard-based partial order reduction, symbolic model checking, ...)
to be used for B and Event-B machines.

#### Unfinished Business

There is a formalism named CSP||B.
Roughly, the idea is that a CSP specification (see, e.g., [Roscoe's excellence reference](https://ora.ox.ac.uk/objects/uuid:d5a3ae86-626e-4387-94f1-070f522040aa/download_file?file_format=application%2Fpdf&safe_filename=68b.pdf&type_of_work=Book) is used in order to specify the order of operation calls of a B machine, that is typically more non-deterministic.

The overall goal is to extend LTSmin's B frontend to also extend CSP||B.
A first work might only consider CSP specifications.
Some re-writes might have to be implemented in order to gain the optimal performance from symbolic algorithm or partial order reduction.
The idea is that by pulling all parallel (||) or interleaving operators (|||) as much to the root of the syntax tree as possible,
one can locate more independence between processes.

#### References

- Jens Bendisposto, Philipp Körner, Michael Leuschel, Jeroen Meijer, Jaco van de Pol, Helen Treharne, and Jorden Whitefield.
  Symbolic Reachability Analysis of B through ProB and LTSmin. 
  In Proceedings iFM (International Conference on integrated Formal Methods), volume 9681 of Lecture Notes in Computer Science, pages 275–291. Springer, 2016.
- Philipp Körner, Jeroen Meijer, and Michael Leuschel.
  State-of-the-Art Model Checking for B and Event-B Using ProB and LTSmin.
  In Proceedings iFM (International Conference on integrated Formal Methods), volume 11023 of Lecture Notes in Computer Science, pages 275–295. Springer, 2018.



### lisb

lisb is an embedding of B in Clojure.
The main idea is that one can specify a B machine in Lisp-like syntax (internal DSL) in order to generate a pure data representation (IR) of this machine.
This IR can be translated to a Java AST that is generated by ProB's parser tool suite.

At first glance, this approach might seem to be entirely useless from a modeller's point of view.
Why should we learn another language / syntax in order to express B?

The main values lies in the potential for DSLs and tool support.
For example, Jan Roßbach developed an [automatic refinement tool](theses/janrossbach/thesis.pdf) that translated sets that are detected as finite during a static analysis to a number of boolean variables (aka bit blasting),
and also splices operations if there is only a finite number of possible parameters values.
This helped in improving performance of partial order reduction.


#### Unfinished Business

There are several categories of work that could be done as (part of) a thesis:

- Extensions regarding the Event-B integration:
  - Theories:
    Rodin has a [theory plug-in](https://wiki.event-b.org/index.php/Theory_Plug-in). 
    The task here would be to design an intermediate language, that generates a theory file. For this, one has to figure out the structure of these files.
    Second, a requirement is a nicer front-end language that generates the IR.
  - Proofs:
    The discharged POs are stored in Rodin as well as the corresponding proofs.
    It would be nice to (a) generate proofs for certain constructs where the required steps are known (e.g., certain data refinements),
    and (b) to translate such proofs to/from other proving tools (Isabelle/HOL, Coq, Lean, ...).
  - Translations from/to B:
    One can figure out the remainder of the translations to fully cover B and Event-B.
- Language Frontends:
  There are a lot of programming languages and formal specifications languages one could add as frontends to lisb.
  A selection is:
  - Solidity (reserved)
  - TLA+ (reserved)
  - VDM-SL
  - PDDL
  - ...
- Misc. Techniques:
  - Translations via clojure.core.logic:
    A proof of concept of the translation from the B DSL to the IR was developed in the bachelor's thesis by Mounira Kassous.
    It would be nice to explore (a) whether a translation from the IR to the Java AST is possible or
    (b) whether IR-to-IR transformations are reasonable (e.g. Jan's refinement tool).
  - Coupling front- and backend:
    It would be nice to link concepts from DSLs as meta-data to the actual code in the backend.
    For example, an if-then-else statement would translate to (at least) two different events in Event-B.
    The goal would be to, for example, link one events with the then-Branch and one with the else-Branch.
  - Specs, Fuzzing, test.check:
    One should overhaul the existing specs and use them for automatic testing.
  - IR-to-IR-transformations / IR analysis:
    One could implement a wide selection of code analysis or transformations on top of the IR.


(older ideas below, possible duplicates)

- DSLs: One vision is that some (any) programming language (or specification language) can simply be transformed in a B model and verified (or even fixed by [B synthesis](https://doi.org/10.1007/978-3-319-98938-9_20) and a minimal change is translated back).
  For this, one needs to explore whether and how concepts such as polymorphism, inheritence and recursion can be expressed in B in a way that is still compatible with tools.
  A concrete example is to translate (a subset of) Solidity contracts to B and verify that no attack is possible.
  The goal here is to determine that the hacked DAO implementation is incorrect.
- Backends: The other end of lisb is the backend. We can translate the IR - which is loosely based on B's foundation, i.e., set theory - to other formalisms or programming languages.
  Julius Armbrüster worked on generating Event-B projects used in the Rodin tool instead.
  Here, a large variety of tools is possible.
- Rodin: As lisb allows easy implementation of, e.g., refactoring tools, one possible task is to create a Plug-In for the Eclipse-based [Rodin](https://sourceforge.net/projects/rodin-b-sharp/)-Tool.
  The goal would be to gain "smart" context menus that are easily extendable. Currently, a refactoring for re-naming a (local) variable is implemented; this should be integrated.
  However, further useful refactoring operations should be identified and prepared.
- Rodin/Theories: The [Theory Plug-In](https://wiki.event-b.org/index.php/Theory_Plug-in) for Event-B allows extension of the language by so-called theories.
  For example, there are theories that add real numbers to the language. One can also add theories for custom, recursive data types (e.g., lists, trees, ...).
  However, the editor is quite cumbersome and, in some instances, very slow.
  One could generate the theory-files from lisb instead.


#### References

- Philipp Körner and Florian Mager. An Embedding of B in Clojure. In Companion Proceedings MODELS (International Conference on Model Driven Engineering Languages and Systems: Companion Proceedings), page 598–606. ACM, 2022

