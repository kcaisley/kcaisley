# Scope

Understanding of chip scale electronics, including sensors, circuits, and the microwave design theory needed to characterize IO.

But this repository needs to be build with a purpose: Designing a PLL and VCO front end in 28nm/65nm + teaching/designing the lab component of a detector signal processing course.
This repo isn't my research, but instead serve as a supporting repo for all the prerequiresit background knowledge and theory needed to be productive in the field.
Knowledge should be partition by theory/discipline, and application.
This repo makes the assumption the reader is familiar with: physics (devices, particle, EM), math (calc, stats, linear algebra, DEQ), and computing (programming, Unix)

Signal and Systems

Devices (follow on semiconductor physics) From junctions to useful devices, i.e. MOSFET, PIN diode, detectors, etcs

Microwave and Optics Primarily useful for knowing when lumped element and linear electronics breakdown. Waveguides, transmission lines, etc. Also not applicable to quantized particle models.



My core knowledge focus is on the design of physical systems which guide electromagnetic waves/photon particles around in order to measure incident particles. This requires a 

My knowledge can be broken into:

**Base level devices** (Active and passive), constructed in monolithic and PCB levels of integration, the physics needed to describe them (EM, semiconductor, particle) the fabrication method for them, and the simulation needed to design them (FEM, process sims, dynamic charge sims, )

**System integration**, fabrication, and packaging. Understanding the capabiliites of different machines, and  

**Engineering Paradigms/disciplines** are just models, but they are a collection of widely applicable, generalizable and composable models that merit being studied together formally.

- (lower level physics based models)
* Device Terminal IV models. Models for active devices are non-linear.
* Lumped Element Circuit Models, linearized, no signal prop time. Ohm, Kirchhoffs Laws, Nodal and Mesh, Circuit reduction, Open/Short circuit tests, AC circuits and phasors, 
- Distributed Circuits Models (due to ratio of wavelength to circuit dimensions, wave propagation time can't be ignored). Used in Microwave and Optics, etc. S-parameters. Z-Parameters. Waveguides, coupled lines, stubs, cascaded lines, impedance discontinuities, power divider, filter, circulator, active amplifiers.
- Digital circuits, perhaps with propagation delay and setup hold time added. Noise margin eliminates noise. Logic gates, expressions, truth tables, state machines, combinational and sequential, Kanaugh maps. Can use HDLs.
- Linear Time invariant systems - an extension/abstraction of lumped element circuit theory. Everything must be normalized to current or voltage. No concept of operating point. S-Domain/Laplace transform, Dirac delta, is covered. Filters, feedback, state space
- Discrete Time Invariant Systems - abstraction of digital, assuming prop delay and other timing violations won't be an issue.


Where do I fit noise into these models? It’s not mechanistic, and so that is a

At the macroscopic systems level, most blocks can't be properly defined by any one of these models alone. This is because of various sources of nonlinearity, noise, and time-variance. For example:

- ADCs and DACs are often wildly nonlinear, and so have various specs like INL, DNL, gain error, offset error, etc.
- RF blocks will have Noise Figure (NF), IP2, IP3, etc to characterize power saturation (gain compression, not tied to frequency, and IM2 and IM3 which are intermodulation for harmonic distortion.
- PLL will have phases noise, jitter, 
- Pixel detectors have ENC, time walk, dead time, etc!
