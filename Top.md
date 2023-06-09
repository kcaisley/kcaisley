Everything is a model, some are useful some of the  time.

The foundations of electrical engineering are math, statistics, computing, and physics/chemistry. We then build the electric engineering devices, systems, and models on top.

## Math
Math is the pure study of numbers, symbols, spaces, and geometries. We can apply it for the construction of models, as well as the optimization of systems described and explained by those models. The relevant fields are 
- calculus (vector, multivariable, sequences and series)
- differential equations (ordinary and PDEs, Fourier series is solving 2nd order ODE, basic theory of Fourier series is infinite dimensional vector spaces)
- linear algebra (vector spaces)

On applying mathematics to engineering: https://www.jstor.org/stable/2309339

## Statistics
Concerning the collection, organization, analysis, interpretation, and presentation of data. The opposite of mechanistic modeling. Used to learn, describe, and predict the behavior of systems after empirical data has been collected. 

Statistical or correlation studies often bypass the need for causality and focus exclusively on prediction, and not mechanistic explanations of “why?”.

As most data is computer generated (Ninety per cent of the world's data have been generated in the last 5 years), these methods are often closely intertwined with computer algorithms.

## Computing
Older methods, like analog computers/slides rules are approximate because of error accumulation from mechanical tolerances and electronic noise. And so the methods explored by them accumulate error in a different manner.

Digital computers instead accumulate error through quantization. Herein lies the beauty of digital; after conversion, it suppress analog noise/error with noise margin, at the expense of quantization error. Note: Computers drastically accelerate numerical **AND** symbolic methods. Therefore, make sure you don’t confuse numerical(approximate) methods are being the exclusive or sole domain of digital computers. Humans can also do numerical and symbolic computation. (Is analog computation a third type?)

Recall: Algorithms, numerical/approximate methods, which existed before Computer science and programming.

## [[Physics]]
Before considering my field, I must consider the wide range of models used to explain the “why” of electronics and particles. These are two low level to be tractable for design, but are important for my intuition and reasoning.


## [[Electronics]]
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