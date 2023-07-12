
* Would the ExternalModules tools be the correct avenue for supporting Verilog-A models of devices? And let's say that we want to, at the system level, mix and match between transistor-level and behavioral modles of say, an amplifier. How would this be best accommodated in HDL21.
    * To answer this, I need to understand what the netlist input to Spectre looks like, when mixed VA and SPICE netlists are fed in.
* Modules are created either via a:
    * procedural approach, where an object is created by passing passing a 
    * or class based definition, which uses a decorate. This feeds a user defined class into the decorator function, and spawns and output function?

* External modules can be used to include behavioral models, like Verilog and Verilog A.
* HDL21 is structural, and not behavioral at all (behavioral element comes from transistor models)
    * But that is how SPICE works too (raw, not including XSPICE, etc). It's essentially all 
    * Bundles are structered connections

* Layout21: Layered Rust libraries for layout
    * tetris - structured grid
    * raw - full custom
    * parsers for industry standard formats
    * It is a library, doesn't have a real user interface, and won't.
        * Look up 'Substrate' used in SRAM compiler project.

Other new integrations:
* Intefacing with ALIGN
* CktGym

Coming to VSCode:
* schematics
* rendering virtuoso schematics in vscode
* simulation testbench setup?

* Low hanging fruit: simulation modes matrix, basic examples -> will open starter issues


* Note how HDL21 doesn't address the layout implementation of cells. There's no strong notion of a layout cell paired with a corresponding schematic.
    * I might choose to use Layout21 libraies w/ tooling on top to accomplish this portion, or I might instead choose to use a mix of:
        * GDSFactory (cell)
        * Laygo2 (structural)
        * BAG+Xbase (cell + structural)
        * Klayout (structural)
    * This is a bit unfortunate, of course, as a large portion of my work will require silicon tested design.
    * I might like to customize the HDL21 flow a bit, so that the generation of layout cells is done in tandom with schematic cells.
    * This would be done before the structural level work is completed.
    * Also, I should like some way to exchange between netlist+model vs behavioral representations of systems.
    * Perhaps I do this by simply having a parrallel set of python classes, build on something like OpenVAF?

# Questions

* Best way to communicate? 
* What was added in HDL21 v4?