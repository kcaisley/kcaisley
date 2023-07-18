# Steps

- Create basic VCO generator
- Work through Ravazi PLL book, comparing against real circuits
- Implement noise simulation via Spectre in HDL21
- Work on physical implementation in SKY130, 65nm, and 28nm
- Short feedback loops in sharing the work.

By the end of this week, I want to have a simulation of a VCO, in 130nm SKYWATER. I want to run it against Spectre, as I want to plot large signal noise, in an eye diagram. Contribute that code as a Pull request. Start from gated ring oscillator example provided by examples. 

Don't do anything that would require having visual access to cadence yet. This includes creating a parallel design in Cadence, creating layouts, or creating images showing how the graphical component of design normally works. 

# Generators, Hdl21, and PLLs Presentation

- Show how design methodology is done in both Cadence virtuoso and Generators
    - Just do schematic based design, as layout would be too time consuming for the next two weeks.
- Generators, in general
    - show old schematics, how hard they are to read
    - show my 28nm layouts -> see how regularized they are?
- Hdl21 (bill it as the next generation of BAG)
    - What schematics typically look like
- VCO design theory? (not extensively)
    - Just show some design equations, for noise, oscillation frequency, etc

- Generating for Both 28nm and 65nm TSMC.

https://github.com/aviralpandey/CT-DS-ADC_generator/blob/main/characterize_technology.py

https://github.com/aviralpandey/CT-DS-ADC_generator/blob/main/database_query.py