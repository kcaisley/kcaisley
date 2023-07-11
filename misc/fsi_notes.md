# General
tparry: his new company, their jobs, and why not BAG, and making a new AALT framework. using Container? What is your level of intent, and why not use BAG, etc?
hpretrl: containers Docker and Singularity, BAG2/3 work
matthias: klayout integration with BAG?
dfritchman: what is your job description, recommendation on readiness of VLSIR/HDL21
Staf Verhaegen: 

Whoah, 3D views of small cells are super cool to see and visualize how did you make them?

I saw that noise simulation is not possible in open source. No large signal noise simulation in ngpsice and xyce, which is necessary for characterization of large signal non-linear operating blocks like comparators.

When I'm designing data converters, the most challenging part for me is always predicting the system level INL/DNL from the block level 

For top level mixed-signal simulation, you can convert Verilog -> Synthesis -> Qflow -> Xspice primitives and run a NSpice simulation.

You said unify layout and schematic; can you explain a bit more?