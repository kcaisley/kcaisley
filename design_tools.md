## Open Source Circuit Design Tool Chain

Wherever possible, specific design instances should be produced with text-based generators, rather than manually created.

Make sure my flow follows a 'top-down design' patten. Then you just fill in the top-down design with flexible generators, in a process guided by and testing constantly against the top level design. *Most generators I see are very much bottom-up, but this isn't the way to proceed all the way to a finished chip.*

Therefore, we need one top simulator workflow where we can mix and match different types of analytic/symbolic (latter is more precise), stochastic, deterministic, measured data, emprical, SPICE/device, extracted TCAD, electromagnetic, and plug them into a simulator that can describe the behavior of the system that can be mixed-signal in nature. Can Verilog AMS handle this?

I want to be able to work entirely in VSCode / Jupyter Notebook when designing my chips. All I should need is my MacBook. My productive work output should exist entirely as a self contained code base, with nothing exterior. My design should be a stand alone generator script.

The high level design languages I work in should be compact enough for me to have them fit in-line with a JupyterNotebook.

I want all of my research & studying, however, to occur organically and manually: with paper and pencil, notebook, textbook, and printed journal articles.

Nearly every real-world chip design starts with some existing framework around you, with an existing mixture of Fab PDKs data, Verilog, Verilog-A, a PCB file, a wireline Touchstone file, a TCAD extracted model, Virtuoso Schematics, Virtuoso Layouts, GDS, netlists, etc. How can I work through incorporating these?

Start design at top level. Specify pin-accurate top level model, the important top level parameters and define the types of simulations, and the imp

The architecture I want to therefore follow is that every step of circuit creation should be specified by some 

## November 8th

I've had some realizations, which is that for building high level schematics, no SPICE netlist is needed for simulation.... Verilog-A can completely capture the connectivity.

Also, at a low level, the BSIM compact devices models these days are even represented by Verilog-AMS now (and not C code).

However, we still have a SPICE netlist dependency when we go to create a physical layout, as we need to run LVS, and this isn't possible unless there is a 1-1 correspondance between the devices in the layout and those in the netlist. Therefore, it's obvious that we need a workflow where we use locked-in Verilog-A models as the starting point for specifying our designs.

Also, BAG relys on having a schematic template from which to work from, so this an additional reason why a netlist/schematic is a necessary step to be produced during my design.

[Reading this article about](https://blog.ouseful.info/2018/08/07/an-easier-approach-to-electrical-circuit-diagram-generation-lcapy/) the package Lcapy, as listed below, we find that it doesn't fit our use case. We don't want to ever specify our circuits in a format that can't be fed into a SPICE class simulator. It's features of symbolic analysis are nice, but we need to restrict ourselves to Verilog-A for this work. 

CircuiTikz

[Lcapy](https://lcapy.readthedocs.io/en/latest/index.html): Lcapy is a Python package for linear circuit analysis. It uses SymPy for symbolic mathematics. The circuit is described using netlists, similar to SPICE, and can also manipulate continuous-time and discrete-time expressions. Has support for generating Latex & Jupyter Notebooks diagrams with CircuiTikz package.

[This article](https://blog.ouseful.info/2018/08/07/reproducible-diagram-generators-first-thoughts-on-electrical-circuit-diagrams/) by the same author above talks about the lower level circuiTikZ package, but this is just plain awful to work in. 

[This article on the PySpice](https://pyspice.fabrice-salvaire.fr/releases/v1.6/example-introduction.html) docs site discusses a couple different strategies for creating schematics. They mention SKiDL and Schemdraw, as options for schematic germination. This is important as PySpice doesn't generate netlists for you.

[Here are some advanced examples of analog simulation with PySpice.](https://github.com/PyLCARS/PySpiceExsamples/blob/master/MosfetTopologies/AnalogMOSFET_Tobologies.ipynb)

Also,  it mentions Pyterate and Pweave, which are simpler options to Jupyter notebook, that don't rely on a complicated XML format, which allows for source control.

[Pyterate](https://fabricesalvaire.github.io/Pyterate/) is a Sphinx add-on that can convert the output of a python file with in-line components into a .html, .pdf, or markdown file. It's not particularly interactive, but is lightweight, fast, and plaintext. It's written by author of PySpice.

[Pweave](https://mpastell.com/pweave/) appears to be similar to Pyterate, but it has some fancier features by using a real Jupyter kernel, like caching all code to make subsequent runs faster. It can actually even output to a .ipynb or sphinx document, along with .html, .pdf, or .md.

[SchemDraw](https://schemdraw.readthedocs.io/en/latest/index.html): Python package for producing high quality diagrams, and uses the backends of Matplotlib (great for use with Jupyter Notebook) or SVG (for use in LaTex -> PDF). It's cool, but again, you're specifying the design in a language written link this:

```python
with schemdraw.Drawing() as d:
    d += elm.Resistor().label('100KΩ')
    d += elm.Capacitor().down().label('0.1μF', loc='bottom')
    d += elm.Line().left()
    d += elm.Ground()
    d += elm.SourceV().up().label('10V')
```

This is super cool, but again, I really need to use Verilog-A and SPICE netlists as my "source of truth" when generating schematics.

Finally [SKiDL](https://github.com/devbisme/skidl), which lets you compactly describe the interconnection of electronic circuits and components. The resulting Python program performs electrical rules checking for common mistakes and outputs a full SPICE netlist.

![SKiDL + PyCircuit + Ngspice + matplotlib data flow.](https://raw.githubusercontent.com/devbisme/skidl/0d3cf9dca19a5fc4d58308d1872f1ed78363adfa/examples/spice-sim-intro/dataflow.png)

[Image Source](https://github.com/devbisme/skidl/blob/master/examples/spice-sim-intro/spice-sim-intro.ipynb)

Example code:

 ```python
 from skidl import *
 
 # Create part templates.
 q = Part("Device", "Q_PNP_CBE", dest=TEMPLATE)
 r = Part("Device", "R", dest=TEMPLATE)
 
 # Create nets.
 gnd, vcc = Net("GND"), Net("VCC")
 a, b, a_and_b = Net("A"), Net("B"), Net("A_AND_B")
 
 # Instantiate parts.
 gndt = Part("power", "GND")             # Ground terminal.
 vcct = Part("power", "VCC")             # Power terminal.
 q1, q2 = q(2)                           # Two transistors.
 r1, r2, r3, r4, r5 = r(5, value="10K")  # Five 10K resistors.
 
 # Make connections between parts.
 a & r1 & q1["B C"] & r4 & q2["B C"] & a_and_b & r5 & gnd
 b & r2 & q1["B"]
 q1["C"] & r3 & gnd
 vcc += q1["E"], q2["E"], vcct
 gnd += gndt
 
 generate_netlist() #Can generate for SPICE, KiCAD, etc
 ```

The future work page of [PySPICE](https://pyspice.fabrice-salvaire.fr/releases/v1.5/example-introduction.html) mentioned a pure python simulator called [ahkab](https://ahkab.readthedocs.io/en/latest/help/Netlist-Syntax.html#the-netlist-file). It's not going to compete with Spectre, but it's very interesting.

### Conclusions

The link I'm trying to establish is the ability to write a Verilog-A model, use it as a starting point to built a SPICE netlist, and at any point, from either the netlist or the Verilog-A model, generate a good looking schematic which can be place inline in Jupyter notebook, or eventually rendered as a SVG/PDF and placed in a LaTex document. Critically, the schematic of each circuit block should represent the content of the block, and not be the high-level symbol of the block itself. At the high level implementation of the system, the design loop will look like this:

> Verilog-AMS model w/ hard-coded in-cell specs -> VAMS testbench w/ plots and specs measurement-> Play w/ simulation to learn needed specs -> Parameterize VAMS model -> Solve for parameters needed for given specs with optimization loop

At the low level now, when making primitives cells there isn't likely any need to have schematic of the behavioral model of the cell, and so the design flow should look like this:

> VAMS High-level parameters becomes Specs file -> Netlist(s) (Template(s)) -> Schematic (w/ no sizes) -> Pick init sizes and run Layout Generator -> Parasitic Extraction -> VAMS high-level model becomes Test Bench -> Play with Simulation -> 

I think that Python doesn't have that much use in the high-level design flow, but in the low-level flow it may have a shot. I was thinking that I should write a mark-up for SPICE netlists which allows schematics to be produced from it. But this forces me to work in a low level language, and then also to write in some form of markup.

A better approach would be to express my circuit in a simpler, high-level manner, which can generate both a SPICE netlist **and** a schematic. This could be where SKiDL comes in. So... maybe....

* Behavioral
  * Verilog-AMS and Verilog-D stays as is. 
  * BAG / PySpice (with Matplotlib) can replace the need for ADE
  * Spectre/AMS Designer stays as is, so I can run Verilog-AMS and Verilog
* Implementation
  * SKIDL / PySpice replace schematic drawing (artistic or functional .OA) and netlist generation
  * BAG / PySpice (with Matplotlib) can replace the need for ADE
  * Spectre/AMS Designer stays as is, so I can run Verilog-AMS, Verilog, SPICE, and PEX netlists all in one environment.
  * BAG or LayGO2 replace the work of layout tools.
  * Calibre DRC / LVS / PEX stay as is



ADMS: Can translate some portions of the Verilog-a language into C code, but I think it's limited to the continuous-time subset of the language. Some of the discrete time components aren't supported, for example (I think).

[Xyce](https://xyce.sandia.gov) 

Ngspice

XSPICE: Add a subset of discrete time primitives into the SPICE language, but it doesn't support Verilog-D, which is a huge problem, as this is what is used for the actual design. At most we could use this for some modeling in test benches, but ultimately this isn't useful.

MagicVLSI: Old school layout tool which has a command line DRC, LVS, and PEX option.

[laygo2](https://laygo2.github.io/overview.html): 

BAG3

Klayout: More mode

Verilog-AMS



Others:

... the entire digital flow??

Verilog, Chisel, yosys, Verilator, CocoTB, 

Charon: Open source TCAD simulation



In order to learn more about the type of Spectre/SPICE simulations.

`/cadence/cadence/SPECTRE161/bin/spectre -help pss | less` (or qpac, etc)



# Literate Programming

[Literate programming](https://en.wikipedia.org/wiki/Literate_programming) is a programing paradigm where instead of generating documentation from well structured comments in your code base, you instead generate your code from well structure snippets in your documentation. Tools like Sphinx are not organic or intuitive, they are mechanical and machine-like, and understanding the internal logic of the design isn't easy.

Instead, literate programming leads the new reader almost step-by-step through the code-base, presenting the code almost in a book format. The generated output of the literate source files are both the pure machine source code, as well as the 'woven' documentation.

The literate programming file uses tags, to separate markdown docs from code. When you first show a subset of a larger function, you can create a macro for it, so that you don't have to manually copy that same code later when showing the 'complete file.'

Pweave is one contendor for this literate programming flow, as you write Python code inline with markdown. The literate source is plaintext.

Jupyter notebook is the most popular option, but the source isn't plaintext. I also can't use the web interface for Jupyter notebooks by themselves if I want debugging and linting features. As it's not plaintext, I can't write it in Vim. But I can write them in VSCode, as it has a built in Jupyter kernel to support opening notebooks.

Additionally, if I want to weave the Jupyter source into .html documentation, or tangle it into source code, I can use [nbdev](https://github.blog/2020-11-20-nbdev-a-literate-programming-environment-that-democratizes-software-engineering-best-practices/) from Github/FastAI.

The main problem I see with Jupyter notebooks is that they're aren't really documents that 'contain complete programs', but rather contain snippets... According to [this](https://slott56.github.io/PyLit-3/_build/html/literate-programming.html) source, the requirements for a true literate programming experience are:

1. Code and extended, detailed comments are intermingled.
2. The code sections can be written in whatever order is best for people to understand, and are re-ordered automatically when the computer needs to run the program.
3. The program and its documentation can be handsomely typeset into a single article that explains the program and how it works. Indices and cross-references are generated automatically.

[PyLit](https://github.com/gmilde/PyLit) and [Pweave](https://github.com/mpastell/Pweave) are both fairly uncommonly used. Compare this to [nbdev](https://github.blog/2020-11-20-nbdev-a-literate-programming-environment-that-democratizes-software-engineering-best-practices/), which has over 10x the number of stars (> 4k). Jupyter notebooks are so common, that it doesn't make any sense for me to choose anything else. Plus, I can view my diagrams interactively in the notebook. There is a reason the Code-a-chip challenge requested that submissions be in the Jupyter notebook format.

I think I might be able to just write in Pweave, but output to a Jupyter notebook format, or just feed to a Jupyter kernel for actual interpretation.

Also, I have to work with languages like Verilog-AMS. Does Pweave support this?


## Why I don't like Jupyter Notebooks:
[Link](https://docs.google.com/presentation/d/1n2RlMdmv1p25Xy5thJUhkKGvjtV-dkAIsUXP-AL4ffI/edit#slide=id.g37ce315c78_0_76)
The good: Inline markdown, and inline images
The bad:
* unknown states if you don't run notebook from beginning, so not really interactive in a trustworthy way
* almost impossible to write tests for your code
* no way to easily import some code into another, as a module
* isn't really that much 'faster' than using a code editor
* you lose many of the helpful features of a texteditor
	* autocomplete isn't fast, requires reruning cells, and recommends local files first?
	* no docstrings
	* no easy way to benefit from type annotations
	* no built-in linter
* shared states make the repoducibility crisis even worse
* jupyter notebook also makes code poorly factored
* jupyter notebooks don't track dependencies well at all...
* no way to copy inputs and outputs into slack, for example
* a whole pile of runnable code is not a good tutorial
* can't break class or function definitions up to aid in explanation


Instead, we can get the similar benefit by just using a top level run_experiment.py and then simply add on a requirments.txt, a load_data.py, a model.py, unit tests, and documentation.

## Literate Programming, Again

I've decided that the best way to provide a literate programming framework it to have markdown format, where the code is in fenced off blocks, with instructions about how they will be executed/interpreted (which language, etc).



This is how [`mdsh`](https://github.com/bashup/mdsh#processing-non-shell-languages) handles it, a multi-lingual tool. I like how it supports both bash and python, but I don't think this will allow things like documentation weaving for .html, or inline plots from python. The file is prefixed by `\#!/usr/bin/env mdsh` so users don't have to do anything special when running it. Any non-shell language (i.e. not Bash/Zsh) like Python or C will be executed by running 

````
#!/usr/bin/env mdsh

# Hello World in Python

The following code block is executed at compile time (due to the `@mdsh`).
(The first word on the opening line could be `shell` or `sh` or anything
else, as long as the second word is `@mdsh`.)

```bash @mdsh
mdsh-lang-python() { python; }
```

Now that we've defined a language handler for `python`, this next code
block is translated to shell code that runs python with the block's
contents on stdin:

```python
print("hello world!")
```
````

Another option is [`knot`](https://github.com/mqsoh/knot) which is written in Erlang, and seems to only do the 'tangling' component of literate programming. The argument is that the hosting repos already will convert markdown into a rendered webpage, so you actually achieve better reproducibility by not having a  built-in 'weave' function.

I don't think this is for me though, as I do really want to be able to show the outputs of the code in the results. As I'm describing a design process, I need the document to actually include the results of my process.

Yet another option is [`lmt`](https://github.com/driusan/lmt), written in Go, which is almost identical to `knot` as it is language-agnostic, and only provides the tangling component of the literate programing workflow. In each fenced code block, you must specify the language and the file name/relatie path to export to.

#### Quarto:

This is my tool!!!!

https://quarto.org/docs/get-started/hello/text-editor.html

## November 2

Designs should be written in such a way that they reduce complexity. I always need to put myself in the position of how other engineers are going to see my work output.

Therefore, I should be careful about my work product is organized, how it is documented, and the number of things that a person needs to know as a prerequisite in order to work on my code.

For example, creating a complete generator tool, that has too many interconnected loops and optimizations is just going to become something another person can't use.

This is one reason why I like the idea of a JupyterNotebook, which can provide a linear path through the work that I do.

Object orientation is good when describing a CAD design process. But I need to be careful as it's likely that if I start recording my designs as a complex template-based generator system, that few people are going to be able to pick up where I left off.

Is there someway that I can specify the design intent of a circuit, without having to rely on templates?

> With OOP-inflected programming languages, computer software becomes more verbose, less readable, less descriptive, and harder to modify and maintain. Quote: [Richard Mansfield](http://www.4js.com/files/documents/products/genero/WhitePaperHasOOPFailed.pdf)

A very abstract and complex description of hardware will also make it harder to maintain.

The thing is, good engineers can look at a transistor level schematic or at a completed layout file and will be able to intuit the purpose and performance of the design.

I don't like schematics that have a bunch of variables in them. This increases abstraction in a way that I very much dislike. I want a very much "functional" circuit design, where specifications and a diagram of the circuit are fed together to produce a completed layout that can be extracted.

In short, I want a certain representation of the circuit to be consistent in the level of discipline it follows.

I don't mind if a topology template schematic produces a sized schematic as it's output, as this is a clear assembly line process in my head. I can "trust but verify" the black box of a circuit generator, as it's obvious to me the design intent, with the specifications.

A physical object of any kind does not inherently explain why a thing is the way it is. My idea is that the intent of the design needs to be recorded in a fashion that actually shows how the intent was translated into the implementation.

> The implementation of a design is never suffient explanation for why that thing is the way it is. And simply adding 'comments' onto something isn't always the correct fix.

Recognizing that the design process (procedural) is seperate from the design product itself (clean, authoratative instance, without legacy), we can 

## November 16

Programming is a design discipline.

You will figure out what you're build as you build it, you won't know exactly what you need until you actually built it. The spec is always wrong, it doesn't matter how fleshed out it is. The only complete description of a system is it's source. And even when it's right, it's still wrong, as systems change over time. Therefore:

> Debuggability > Correctness by construction

Programming is actually mostly debugging. 

Batch mode programming still dominates the majority of programming, and this is not great. There is no interactivity while programming. Let's examing a few languages:

C++: completely unprincipled. Three as so many ways to complete a different task, and the language has so many ideas which aren't compatible with each other, that everybody who uses it essentially just needs to pick a certain subset of the language and limit themselves to that if they hope to be productive.

Javascript: Same thing happening as C++; tons of new features being added, fairly disorganized. 

Go: Go-routines and garbage collectors are nice, but it's really just another punch-card batch processing complile and run language. Also not a good library language, as will always need to bundle the go run-time.

Rust: Good library language, but again is a punch-card type system. But it's super complicated, and as it's a compile and test language, you can't easily understand that complication. Stop-the-world garbage collection is bad, and go solves that, but so is stop-the-world typechecking.

#### So what doesn't suck?

How about systems that read code, and give you a visualization of what it does? You shouldn't have to manually create representations and keep them in sync with your code. Visualization tools should sit cleanly on top of source code, and and allow you to double check it's functionality in a visual manner.

## November 18

I've realized that there is no golden method for circuit design, and that the process of circuit design really is a completely custom and human driven endeavor.

I want to understand at a lower level how primitive models are defined in SPICE (look at Ngspice and tsmc65).... wait I do know how they are defined. If I just look at the < BSIM4 models, I can clearly see that the compact models for devices have been historically written in C (or is it C++?).

Tools like Xyce, Cppsim, adms, and Ngspice are all written in C or C++ as well. Verilator isn't a interpreter for Verilog-D, but instead a complier which converts the high-level HDL to a monolithic simulator executable in C++ (originally C) which is then compiled and executed. Gnucap is as well, written entirely in C++.

This makes clear for me the issues then: Can I somehow write models in Verilog-A, which can be translated into something directly executable by Ngspice? This would be an amazing proposition, but I don't know if SPICE can deal with the sort of discrete time constructs in the superset Verilog-AMS language.

So let's move one step at a time. Let take a look at the base models provided by the Sky130 PDK. Are they BSIM models? How are the PCELLs implemented? How is the radition hardness specified? What sort of digital timing files are included? What format are the Standard Cells in?

#### a closing argument (on November 18)

1. Cadence has invested a lot of though in building it's ecosystem the way it it. Not all of it is good, but I can't know criticize it thoroughly enough to develop a good replacement, until I actually know it's limitations and it's good ideas. The same way a person needs to learn C/C++ before they can build Go, Rust, or Carbon, I need to understand the Cadence workflow with Spectre and Open Access before I can properly make anything.
2. Similarly, I can dream as much as I want about using Julia, but I can't reinvent all of modern chip design myself, in the time frame of a PhD. If I want to start innovating, I need to adopt a fully functional ecosystem now for my current work, and replace things '1 piece at a time, depending on what hurts the most'. If I do this, not only will I be more likely to build something that solves lots of other people's pain points as well, but that thing is more likely to get noticed and used by other people too, as it will plug directly into their fully functional ecosystem, and be a direct improvement, without forcing them to give up their current way of working.
3. In order to build something that is useful, I also need to be a user of the tool that I'm building. I don't clearly know chip design that well, and so to become a truly good tool-maker, I must become expert at the making of the tools themselves.
4. Finally, there are plenty of aspects of the EDA ecosystem that I don't fully understand right now. If I try to build my own tools with my current knowledge, I can easily be wrecked by a single issue - like for example like `Ngspice` not being able to simulate proprietary foundry (like tsmc65?) models, or `Ngspice` not being able to read in Verilog-A code. The better workflow is to, again, start with a fully functional workflow, and use it as a test bench to see which places I can actually replace, vs not. If I try to jump fully in, without testing, I won't get anywhere fast.

In short: 

> Proprietary tools can sometimes prevent you from learning how to think deeply about your design, but if you force yourself to learn how everything core inside it works, you can learn best practices and what works, start to see the cracks in what doesn't, and have a modular test bench to replace and verify your improved custom solutions, without sacrificing enormous hits to your productivity or ostracizing yourself from your groups workflow.
