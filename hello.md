I am developing a collection of circuit synthesis engines which are able to produce complete radiation hardened IP blocks including PLLs, TDCs, and readout pixels signal chains.

Top level design of the system is initially completed in Verilog-A. The trick is that we need to be able to make sure that we write Verilog-A which is actually then synthesizable down into an analog layout.

Each parameter of the high level verilog-A model is something that will serve as a parameter which is then passed down to the low level analog implementation.

The high-level specification will populate the high level 

The clearly parameterized Verilog-A model is the the input to the analog generator, and the output, which is a complete layout alongside an extracted schematic, is then characterized and the absolute values are back propogated to the Verilog-A behavioral model.

28nm and 65nm are going to be the target processes, and one should note that 28nm has a more restricted method of 

1. Developing a Verilog-A model a ROSC VCO, which is then able to be simulated in both Cadence and Xyce and NGSpice. I can only use built-in analysis techniques from Cadence/Xyce/ngspice if they are features that are available across all three, beyond that, my evalutation/post processing pipline should be written in Python. Critically, I shouldn't be trying to predict realistic values for the VCO based on some input parameters, or trying to model the internal structure of the VCO. I just want to have a terminal and pin accurate behavior model.

2. Taking an existing VCO design, which was designed in Spectre, and making in cross platform simulatable. This is necessary because I've never built a VCO before, and I want to be able to be able to verify the precision of my model. Once my model matches the real VCO, including parameters that might matter for which it is nested in a PLL (like phase delay, and phase jitter, and power consumption.)

3. When specifying this model, some of the parameters should be input parameters, and some of them should be 'output' calculated parameters. For example, you can't achieve any artibtray combination of bandwidth/speed, power, and gain in an amplifier. Trying an artibrary combination will likely result in an over constrained specification, and so we should be clear about what are the parameters that become fixed (like frequency) and which parameters are 


Okay, so I want to put together a simple and lightweight chip design environment which is 'IDE aware'. I should look into being inspired by the rust compiler and analyzer. That software apparently works quite well.


In terms of top and bottom up design, I think I still have a log of thinking to do in this regard. However, it appears that properly working with high-level Verilog-A models of devices isn't something that I can make cross platform right now. ADMS is deprecated, and it appears OpenVAF still has a ways to go before being integrated into Xyce, and even in the ngspice integration, doesn't support higher level modeling concepts. It just doesn't low-level compact models, for individual devices. Furthermore, working with Verilog-A models at a high level is less-useful if I don't have a library of components from which I can extra operating parameters. I will struggle to know if my Verilog-A models are accurate, if I can't compare them.

Therefore, what I aim to do is build some simple low-level generators, particularly VCOs to get started. By working in bottom up, I eliminate the need to worry about Verilog-A compatility of my simulators. I can simply generate layouts, using TSMC28 and TSMC65 and then extract their performance characteristics.

I should work inside of Cadence virtuoso, but restrict myself to only using scs netlists, Spectre command line input, and Spectre commandline output. Then I will handle all of the generation of a circuit in Python, and analyze the results in Python. For analysis, I should only use fundamental analysis types in Spectre, and should do all pos processing beyond these in Python, including plotting.

It looks like ngspice has a Spectre and Hspice compatibility mode, and that Xyce support Hspice too, with respect to netlist formats. However, I don't want to spend a bunch of time manually modifying Spectre style netlists into compatibility. 

Therefore, at least for the time being, I want to create my netlists in a Spectre compatible fashion, and verify I'm doing so by using the schematics XL tool, but at least when I start I will only be building a couple different classes of VCO, which are relatively low in transistor count. If I later migrate away from the Spectre netlist format, with OA cells, I won't have made a huge investment.

Actually, on second though, I think I should just write raw netlist files, and not extract them from a schematic GUI. Is this even possible, if I want to use the BAG primitive template transistors? My motivation is that I'm working remotely at the moment, and a full gui is very connection intensive. Perhaps Spectre is even flexible when it comes to the netlist format, and so I can write in a non-Spectre format.

I should look at the code of BAG with OA, and try to understand how it is 'removing' the OA dependency. Is the OA code still there? Or has it just been decoupled, such that an a parrallel implementation could be written? I can find these answers on Github, I suppose.

Another thing I still need to understand is how BAG and TSMC PDK would behave differently in creating individual transistor layouts. Does XBase build on the PDK Pcells, and how are these Pcells written? If not, how do the final netlist for simulations gurantee model accuracy? I think I can rewatch the two BAG tutorials on Youtube to figure out the answer to this question.

One reason why VCOs are a great circuit for me to start with is that they are rather small in terms of transistor count, and also have a fairly well defined interface. For a standard VCO, I simply need three amplifiers. Perhaps with an auxilarry biasing circuit. Then I can also try implementing a DCO circuit, which will have a different transfer function type, and my job is to understand how to homogonize them.

Even this relatively simple circuit will have a whole world of optimization for me to learn about. I can get quite sophisticated in how I analyze and construct this device. I could even learn how to write my own optimization routines, so that the generator isn't just feedforward. Something with generations of the design, perturbing around each point to optimize it. I can try building it in different processes, optimizing it for different oscillation frequencies, and try different architectures. Even just this simple VCO/DCO space could take years of my life. We'll try to limit it to within 10 months though. 

Once I figure out how to properly build BAG and start it, I can successfully limit the scope of knowledge that I need to work on. To just build a bottom-up VCO and DCO generator, I can cut out Verilog-A, semicondutor physics, standard cells, Verilog-D, detector physics, Cadence ADE/assembler, mixed signal simulation, etc.

1: Analog circuit design: Razavi Analog, to size transistors and simulate VCO
2: PLL circuit design: Razavi PLL, to understand design/application space, in substition for hands on higher level models and test benches
3: Python Programming: Python docs, Chat gpt, BAG codebase and docs?
4: PDK organization: BSIM, TSMC65/28nm, PCells, DFM files, layers

This is much more servicble.

# March 3

I'm learning about optimization techniques for integrated circuit design. My initially imagined ideas for how the circuits should be designed is turning out to be rather naive.
As post layout simulation is extremely expensive, I can't simply just do random gradient descent with anything other than the simplest of circuits.

It appears that building ML trained models of circuits isn't that feasible, as the simulations necessary to train the model in the first place are so expensive that the model would be quite low quality.

The paper on BagNet explains that a better approach is to build a generator, which can take a wide range of input transistor and generate a DRC and LVS layout, with parasitics extration. But this components shouldn't have any intelligence in it. We then can nest this in an external human-driven or machine-driven optimization process. We shouldn't intermix the generation and evaluation loops.

To a degree, it's okay to limit the range of possible input values to some maximum size, in the case that they would get too large to have a parameterizable layout. Also, it's okay if the layout isn't as compact as it could be. It's not a good use of designer time to optimize generic analog building blocks for size, normally. Usually performace comes a lot more from sizing that metal layer spacing. This is within reason though. The layout plan should be chosen to balance the need for a simpler script, a wide range of possible inputs, and minimized parasitics/silicon area.


