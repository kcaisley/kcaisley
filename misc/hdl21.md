Modules are created either via a:

* procedural approach, where an object is created by passing passing a 
* or class based definition, which uses a decorate. This feeds a user defined class into the decorator function, and spawns and output function?


# Questions

* Would the ExternalModules tools be the correct avenue for supporting VerilogA models of devices? And let's say that we want to, at the system level, mix and match between transistor-level and behavioral modles of say, an amplifier. How would this be best accommodated in HDL21.
    * To answer this, I need to understand what the netlist input to Spectre looks like, when mixed VA and SPICE netlists are fed in.
    * 