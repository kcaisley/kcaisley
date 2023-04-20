# Open Source Analog Design

List of open-source projects tools for analog/mixed-signal circuit design.

* [Verilog-AMS](https://verilogams.com/)
* [BAG]()
* [gdstk]() gdspy successor, written as a C++ extension to python
* [gdsfactory](https://gdsfactory.github.io/gdsfactory/) python stack, based around gdstk, klayout, etc
* [phidl](https://github.com/amccaugh/phidl) legacy layout too, superceded by gdsfactory
* [CppSim](https://www.cppsim.com/)
* [OpenVAF](https://openvaf.semimod.de/) Rust Verilog-A compiler, only supporting compact models (no AMS behavioral yet). [More info.](https://www.youtube.com/watch?v=4Cf00ZeueNc)
* [VerilogAE](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=9193978) Also from Semimod. Python component wrt OpenVAF
* [Devices.jl](https://painterqubits.github.io/Devices.jl/)
* [Cedar EDA](https://cedar-eda.com/) 
* [Klayout]() made by Mattias
* [Xyce](https://xyce.sandia.gov/)
* [Magic VLSI]() Includes 'ext2spice' which is the only open source parasitic extraction tool I know
* [CocoTB]()
* [BSIM models]()
* [EKV models]() now combined with BSIM
* [ngspice]()
* [xbase]()
* [laygo2]()
* [Layout21](https://github.com/dan-fritchman/Layout21) Rust library w/ GDS, LEF, etc support
* [PyCells]()
* [Skywater130 PDK]()
* [IcarusVerilog]()
* [ADMS]() Deprecated, don't use
* [PySpice](https://github.com/PySpice-org/PySpice) Python front-end for controlling Nspice and Xyce
* [SKiDL](https://github.com/devbisme/skidl) Python Library for embedded DSL for schematic entry
* [Gnucap](https://www.gnu.org/software/gnucap/gnucap.html)
* [Verilog]() Also called 'Verilog-D'
* [OpenFASOC]()
* [OpenROAD]()


# Outside of Analog mixed signal design, there is also:
* [Verilator]()
* [Chisel]()
* [Yosys]()
* [OpenFOAM](https://www.openfoam.com/)
* [Modelica](https://en.wikipedia.org/wiki/Modelica)
* [Charon](https://charon.sandia.gov/) semiconductor device modeling code, widely referred to as a TCAD (technology computer-aided design) code, developed at Sandia National Laboratories. It is written in C++ and relies on another Sandia open-source project, Trilinos
* [Elmer](https://www.csc.fi/web/elmer) The only open source open for EM simulation
* [AI-mag](https://github.com/ethz-pes/AI-mag) Technically open source, but relies on MATLAB and COMSOL

# Electromagnetic Simulation:

* [FastCap](https://github.com/ediloren/FastCap2)
* [FastHenry](https://github.com/ediloren/FastHenry2)

[This site](https://www.matecdev.com/posts/differences-fdtd-fem-mom.html) has information about the three basic numerical approaches for electromagnetic simulation, namely:

* Finite-differences-time domain (FDTD)
* Finite Element Method (FEM), like Elmer
* Method of Moments (MoM), or equivalently, Boundary Element Method (BEM)

It also documents the available solver types.



# Documentation

[Handout](https://github.com/danijar/handout) Alternative to Pweave (as it is now outdated), which takes a .py file as an input with Mardown in comments, and writes out to an .html page with images added in. Just refresh the browser to see changes after running.

# My needs

as a chip designer, fundamentally, I think and work in different paradigms:

simple mathematically tractable models:

linear transistor models with linear circuit fundamentals

beyond anything but the most simple circuits, we abstract with

discrete and continuously time transfer functions

OR

some sort of quantized non-linear step based output

OR

in limited cases, a nonlinear expression, in the cases of things like VCO gain error

AND

standard deviation for static offset/mistmatchs and standard deviations for timing amplitude noise

netlist based similations:

using a python based netlist generator, we can pull in real device models to be used in Spectre sim,



# Julia Simulink alternative

DifferentialEquations.jl is great, but very low level and would require writing all eqs myself

Sims.jl is most similar (uses Sundials and DASKR internally)

Modia.jl, from the creators of Modelica, but it's new. Built onf 

RigidBodySim.jl which uses DifferentialEquations.jl inside

ControlSystems.jl covers a fair amount of the controls toolbox

SimJulia is a discrete event simulator

CppSim seems to combine things in a way I want?

ModelingToolkit.jl?

Modelica (C++ modeling toolkit)





# Assembling a Workflow

The scope of this section is the identifying the needs I have have in my research workflow, and what tools I will use to satisfy those needs. First, we will begin by mapping our the sorts of mathematical models I need to tractably work with.



All data should be pulled back up to a high-level programming language. This is because all of my thought and decision making can be done in an an environment that is highly-flexible. Julia has some really nice features including speed, built in arrays and symbolic computing, and inline docs with Weave.jl. But the ecosystem isn't that mature yet. I'm choosing python for the following reasons:

1) Everybody at FTD, Berkeley, and even Lise uses Python. I have a much higher chance of passing knowledge on and getting help if I work in it.
2) Stability
3) Fast parts are fast enough, slow parts hopefully won't matter
4) Composable enough, without clear issues like Julia
5) Huge userbase, and chat gpt uses it
6) More mature tooling, no need to self compile

I can run against low level spice simulators, and then use regression to extract a simple model for high level simulation.

Julia the language is quite beautiful, and the tooling is on paper great, but I'm really not an experience software developer, and I don't plan on writing that many lines of library code myself in the coming years. At least for the next 3 years I will spend more time on the application than on the libaries themself, and so I should tolerate some poor composability of interfaces if it means I get in exchange some more popular libraries which I can understand.

Therefore, I should use Python.


# My use case

There are different solutions of automation in Analogb IC design, and I should be clear about my use case in order to best identify the workflow to choose.

* I am generating for proprietary TSMC 65nm and 28nm PDKs, with a shared NDA between approximately 20 institutions.

* reproducibility: keeping the math and design equations in line with the 
* process portability: design simultaneously for multiple processes
* reduced manual layout work
* Top level modeling, in a language HEP knows.
    * Additionally, if I shun Verilog AMS, my design simulations should be able to run against NgSPICE and Xyce as well. 
    * Also, if I use the concept of "regression" I can build block models which I can use to have a high-level pure python model of my system. This will allow simpler reuse of my system models by physics people who just want to work in python.


