Why does the field of ASIC design not use super computing resources?

[LBNL history of Jupyter and IPython](https://cs.lbl.gov/news-media/news/2021/project-jupyter-a-computer-code-that-transformed-science/)

Design files, PDK data, and tool executables should be able to be bundled and send off to super computer to execute specific operation. Critically though, the Jupyter notebook/programming interface should be able to be run on a local machine.

Store massive data sets in a global file system, and then run the software executables, stored in containers, to be moved around.

Notebooks need to become the interface for developing tools, and for then disseminating knowledge about how those chips were designed.

The key then will be to use Spectre/NGspice/Xyce as a cross-compatible back-end running the design notebook for a chip.

Different graphical viewers, including Virtuoso, Klayout, Xscheme, etc should be used for debugging purposes, but shouldn't be part of the core development flow.

[Scientific Paper is Dead](https://www.theatlantic.com/science/archive/2018/04/the-scientific-paper-is-obsolete/556676/)

Before scientific papers, there was no public (not letters), non-ephemeral (not lectures), incremental (not books), way of documenting science.

These papers were easier to follow, less specialized, and what "computation" was used could easily include data and equations on a single page, and be verified by hand.

The data processing paper now **is** the paper, and despite this, is often left out of the results. Scientists haven't really taken full advantage of computers. They're simply trying to emulate older methods. The fact is, software is a dynamic medium, but paper isn't. Papers are stuck in the past, where we're not able to play with the modern simulated toy models we use to build our works. Why should our readers be stuck with an overly formal, paper only representation of what I'm working on?

Idea is to play with computers, in a carefully authored "computational essay", or notebook. In any field, the prefix of "computational X" is starting to be applied. The fundamental shift in thinking is similar to what happened in the 1600s when people starting to apply mathematical notation to their fields. One thing especially nice about a interactive notebook is that there is no "fudging", the written models inherently completely describe the process.

You know the need for a good new tool when you feel like you're using a hodgepodge of systems, with constant context switching. This limits the ability to be exploratory, and a new tool can unify and fix the work. One should use the computer as a "thinking partner". You should be able to "sketch" out the problem.

> The paper announcing the first confirmed detection of gravitational waves was published in the traditional way, as a PDF, but with a supplemental IPython notebook. The notebook walks through the work that generated every figure in the paper. Anyone who wants to can run the code for themselves, tweaking parts of it as they see fit, playing with the calculations to get a better handle on how each one works. At a certain point in the notebook, it gets to the part where the signal that generated the gravitational waves is processed into sound, and this you can play in your browser, hearing for yourself what the scientists heard first, the bloop of two black holes colliding.

Of course, until journals require notebooks, there will be minimal incentive for any individual to do it, as it will allow others to take their work most easily. Sharing doesn't earn prestige right now.

Often, the people who build good tools have to leave academia for industry, and will sacrifice their academic career in order to build the tools they really want to use. But this is slowly changing.

To build not just good, but **successful** tools, the values of the developers must align with the values of the users. 

[https://www.nature.com/articles/d41586-021-00075-2](Discussion of how software has changed science)

> “We really do have quite phenomenal amounts of computing at our hands today. Trouble is, it still requires thinking.”



















