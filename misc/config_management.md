## Config management basics

I read a [blog](https://blog.nelhage.com/post/declarative-configuration-management/) about configuration management and it explained that Kubernetes strictly defines the layer for defining state of libraries, the intermediate streaming layer which is protocol buffers, and the layer which implements the config on the actual system. This is useful because the decoupling allows a user to use whatever language they prefer and easily refactor/verify the library state generation code. 

Because the constructs of a language are often super well equipped to deal with parameterization and abstraction, you can take full advantage of a language to generate in clever ways.

This same paradigm exists in circuits. We have clearly define inputs and outputs in the various phases of the circuits. We would benefit from standadizing the interfaces of these stages so that we can more easily interchange between them.


# Work Plan

For the time being I want to just start working through designing some circuits at the schematic level. I have a feeling that there is a whole file world out there of computing, algorithms, mathematics, optimization, machine learning, etc. But I can’t even claim to be comfortable with circuit design.

So what I should do is proceed with reading textbooks and solving problem. I want to keep my work in open formats, including schematics, nelists, and simulation plots. This way I can reuse things over and over. I want to be able to call Spice to check my work, but I also want to be able to work through the mathematics myself.

I want to keep my toolset simple, using Numpy, Scipy, Matplotlib, SVG schematics, Handout, and Latex Notes. Overtime I will learn to work with toy problems, but these code snippets will be refactored by me over time to build a large library of useful code. I can target the SKY130 process for my toy problems, and ask people on FOSS for help and review on my problems.

Also, alternating between textbooks, I can probably post lots if blog articles on how I worked through my problem set.
