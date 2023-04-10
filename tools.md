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

[This site](https://www.matecdev.com/posts/differences-fdtd-fem-mom.html) has information about the three basic numerical approaches for electromagnetic simulation, namely:

* Finite-differences-time domain (FDTD)
* Finite Element Method (FEM), like Elmer
* Method of Moments (MoM), or equivalently, Boundary Element Method (BEM)

It also documents the available solver types.
