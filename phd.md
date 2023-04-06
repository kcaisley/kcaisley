# Literature/research review plan

I want to balance 'productivity' with learning while I start to put together my understanding of PLLs. I need to stay realistic though, and give myself tasks which are possible for me to do.

Reading and synthesizing all the information in these papers is going to be an aimless pursuit unless I set clear goals for what I want to get from it. I will start by simply reading the Razavi paper, and then building on it as a baseline.

The approach is simple. Read the razavi paper, read the paper on the PLL







## ? October

Perhaps I have been overly stressing about dreaming big in my PhD. Marco pointed out to me today that Kostas, who was very successful, was assigned a small power of the monolithic chip, but ended up taking over a large portion of the design work once he had demonstrate that he was so prolific.

Some students fail to dream big, and see the full picture of their work. I don't have that issues - some days all I do is drea. If I instead want to be prolific, what I need to do is focus on the task on hand, and simply <b>produce</b> each day.

This is easier to wish for, than it is to implement though... how do I know what can be considered as 'work product'? I actually really do like the idea of, for example, solving one textbook problem per day from my analog circuit textbooks. Is this a good use of my time?

Also, how do I fit in the other things I want to do with my time, like socializing, running, rock climbing, reading, learning languages, travel, and spending time with Lise? This is tricky. One key thing to keep in mind, though, is that there is no difference between this phase of my life, and the rest of it. The habits I build for how I spend my time today will last me for a lifetime.





## 2 October

**Imagine**



One of the disadvantages of a university research group is that the PhD students are continuously coming in an out of the door, and they leave once they are becoming quite qualified.



A small team like this will benefit most, if they can all focus on one central process... but this is tricky as each student will want to publish something that is unique.



Perhaps there is a workflow for groups which can solve this issue, where the professor takes a more hands on approach to managing their students. Making sure each of them understands what they are working on, and that they have some common background.



One should make it such that PhD students can spend as much of their day as necessary working. Part of this should be a guaranteed time during the day where paper reading is completed.



But give students the leeway to suggest improvements to the process! They need to feel ownership.



Most PhD students typically feel overwhelmed by the fact that they are working on a project all alone.



Papers/Publications can be organized into a repository of: ideas, in progress, in writing, published, etc!





**Mixed Signal, Top Down, Design and Verification**



Systems should be modeled and simulated before that can be built. Behavioral modeling of a system requires a language meant for hardware description. This is where Verilog-AMS comes in. Also, these models should be parametric as they will likely be later updated. Also, a good MS-HDL must be able to be synthesized into a hardware implementation, and also should be able to be co-simulated alongside a transistor level netlist.



The simulator that runs this model should not be something home spun. This is where NGSpice or Spectre comes in. It must be trusted, and should be able to be used for cosimulation with spice netlists.





However, testbench control and data analysis also needs to be separate. This is because programmatic languages are better suited for actions like plotting.



## 29 September

the simulation engine that works with my behavioral models should match that of my transistor level models Hence this is why Spectre AMS is necessary. i allows me to mix and match between Compact SPICE transistor modeling, analog behavioral, and Digital verilog.

each transistor level block, should have a corresponding with behavior model. Each component of the system, low test bench will instantiate a different mixture of behavioral and extracted parasitic netlists in order to verify the system.

no testbenching, stimuli, or post processing code, lives in the same database as the design library.


I can do my 65nm designs in xbase, as performance isn't cutting edge, and then I can more easily port to 28nm.

starting tomorrow, what I need to be doing is understanding how to write verilog behavioral A models, and how I can then simulate them.

Don't spend much (or any) time post processing the waveforms with ViVa or the calculator, as I will be doing that in Python.





## 19 September:

I really want to apply for this competition: https://github.com/sscs-ose/sscs-ose-code-a-chip.github.io

Its submission deadline is November 21, which would give me a perfect outlet for completing a functioning Jupyter notebook in a reasonable amount of time. I've already been here for 133 days, and I feel as though I haven't accomplished anything. For now though, don't tell Hans about this conference.

I want to learn how to use BAG, because OpenFASOC isn't really meant to work with analog chip design. The issue is that BAG3 doesn't support open source tools, and that access for the necessary OpenAccess library is apparently very difficult to get anyways.

I want to use BAG3, as BAG2 is not going to be further developed. My main question then is what are the major differences between the two? I'm meeting with Thomas tomorrow, and he seems to understand the better the status of BAG.

So what if I did the following.... built a generator which is able to produce a functioning VCO layout.. or even PLL, and do it in a way that uses the toolchain of BAG3? If I accomplished this, I would be the first person to successfully use BAG3 outside of Berkeley. Other papers, few of which exist, have used BAG2 only.

The issue now, is that I'm not sure if I can really build a functioning tool chain with BAG3. It doesn't support open source tools yet, and getting it to even work with Cadence seems nigh impossible for those outside Berkeley/BlueCheetah. I should confirm this by reading papers, tomorrow, of course. I think I can get through a lot of material by the time I meet with Thomas Parry at 3pm.

One thing I'm realizing though is that a lot of the design process doesn't really require me to have a function BAG installation. XBase seems like it is very tied to BAG, but I think LayGO2 is less BAG-centric? I should really check with JD Han to figure out how I'm expected to produce a layout in 65nm if I don't have this special Cadence OpenAccess library. Perhaps I can w

Mirjana is a member of the committee which decides the winners of the code-a-chip competition, and so if I were to develop a notebook with her guidance, then there is a *very* good chance of being accepted.

Another note, I should consider giving a group presentation on Sep 29th, because I told Hans that I would. Maybe I should break this promise though. I should explain to Hans that understanding the status of all this open source code has taken significantly more time than I anticipated. Explain that I've made connections with two important people though!

If I don't use BAG, then how do I work with the schematics needed to compare against LayGO layouts during LVS?

If I want to design a process portable generator, do I target the layout constraints of a small-fill factor 28nm design, and then expect it to scale to a 65nm, or even 130nm process? I think this makes more sense, as there are less degrees of freedom in 28nm.

Can this tool (LayGO2) be used in isolation, without BAG?  I think it can!

One thing that is amazing about this, is that Laygo is written entirely in Python, and so I shouldn't have any huge problems getting it to run. No compilation required, etc. It's a 1-way path for generation.

Also, much of the design work 'back of the envelope work' can be done in Python scripts, without any circuit simulation at all. But, then, once I get to circuit simulation, I wonder if I am able to run parasitic extraction with Magic on open source Tools on 65nm? If think seems to work well, then I think I can trust it enough to do large cell design without re-running a DRC/LVS ruleset in Calibrew. To be double sure, I can simply run a final Parasitic extraction and DRC and LVS test against Cadence.

There's also no reason why my schematics would necessarily need to be be designed in cadence source tools, right? If I am looking for process portability though, I think that I need to use something like BAG, where I am am able to make schematic and test bench generators.

So one question is: If I'm not using BAG... how do I achieve process portability in my schematics, such that I can quickly compare them against laygo? I think that the basic Laygo workflow uses hand-crafted primitives, which aren't process agnostic. On the other hand, I think that Lay

I can slowly replace every part of the 65nm workflow in Cadence, with equivalents in 65nm technology.

#### BAG Notes:

I think the functionality is primarily good for the Analog Chip Bottom, where the SAR ADC, the Bandgap references, the Analog Multiplexer, and monitor ADCs, the Calibration Injection Voltages, the Serdes, and the CDR and TX/RX Circuits live. This is where documentation, reusability, comprehension, and testability are much, much more important that cutting edge performance.

Generators are also best suited for instances where you will need to do many different iterations, in possilbly different 

BAG3 is a major upgrade, as it reworks the way layouts are generated in the tool. There is no AnalogBase/DigitalBase constructs anymore, just MOSBase.

As long as devices share the same 'row' information, you can tile NMOs, PMOS, and TAPs right next to each other.

Don't specify the width of each wire, have 'wire classes' which have preset widths.

MOSBase just places the drawn transistors, and isn't necessarily DRC clean. MOSBase Wrapper then comes back and fill in the dummy devices, extension regions, and boundaries.

In the YAML file, we have to define the properties of each row. It's more complicated in BAG3.

The layouts may only have one type of tile, or you may have a more complicated multi-tile type layout.

In BAG2 AnalogBase, you only have access to M3 and 4, (maybe more in Digital Base). But in BAG3



Zhaokai Liu showed a BAG3 repository example on Github, but it's a private repo right now, and we can't see it.

Bulk contacts are completed with TAP devices in BAG3.

Different length devices are implemented with stacking devices, to increase effective channel length.

To place transistors, you first specify the tile you are placing in, and then use XY coordinate (e.g.) within the tile. It defaults to position 0 for X and Y.

BAG2 only supports ADEXL. BAG3 doesn't support any ADE package, Assembler, XL, or otherwise. Design variables are returned, but Stimuli and Data post processing must be done in Python, as we are just directly  calling Spectre.

Notice how all the BAG developers are also using BAG to build real circuits.

BAG3 includes BAG2 

Behavioral model of circuits are completed

Marco in Finland



Infineon guys also putting money into it

Mattias is from Klayout





