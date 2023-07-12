
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


# Questions

* Best way to communicate? 
* What was added in HDL21 v4?
