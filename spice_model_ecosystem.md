# April 18, Sadayuki Yoshitomi

Analog work-group meeting



In the tape out stage, we are increasingly needing large design margins at high speed, as real silicon is far from actual PDK models.

PDK models are correct in the sense that they are extracted from Si during development, and yet why do designers claim SPICE models are inaccurate?

![image-20230418181857736](/home/asiclab/notebook/images/image-20230418181857736.png)

There are many different ground-source-ground (GSG) test structures used for calibration of the different properties of the devices, including

1) dielectric constants/well resistance
2) metal conductivity
3) fringe capacitances/metal
4) fringe capacitances/ via

![image-20230418183246467](/home/asiclab/notebook/images/image-20230418183246467.png)



Compact modeling:

Needs to balance accurate modeling across physical and technology variation, while still remaining mathematically tractable (poisson, stochastic LLG, schrodinger, Boltzmann Transport)

There are basically three types of compact models:

* Macro models, using lumped element circuit devices to mimic device behavior
* Table lookup, {I,Q} = F{L,W,T,VD,VG,VD,VB..}, this is of limited value early in device evaluation
* Physics based analytic model, computationally effici

