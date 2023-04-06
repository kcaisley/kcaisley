This is the case for working on a unified,  monolithically integrated, generated PHY interface for pixel detectors, 

1. the research group i am a part of is responsible for producing the three of the most important pixel detector ASIC chipsets in the world, and so it would be madness to try and focus on imaging systems, or systems for XRays. Just like the systems below, I should focus on *massive* particle *detection*
   1. RD53 (ATLAS and CMS)
   2. DepFET ASM/Switcher/DHPT chipset (Belle [and EDET!])
   3. LFmonopix/TJmonopix/Obelix/MALTA (Belle outer PDX, ALICE)
2. On the chips that we haven't completely designed, like RD53, we were in charge of most of the top level integration and the PHY. The AFE wasn't our responsibilty in this case.
3. I don't enjoy full digital design, and the digital cores aren't very interesting or specialized, as the HEP application doesn't change design much. 
4. Analog design for the front end is very competative, and I don't have the skill to compete well with Amanda and all.
5. Alternative, new analog circuit features are not something I'm able to dream up well right now.
6. People aren't that interested in the implementation details of the data readout, but our group specializes in this, and I would have little competition. 
7. We also have the corresponding chip testing infrastructure. Intimate knowledge of the BDAQ system is super useful when deciding how to arhitecture the next generation Readout.
8. Working on a set of 28nm and 65nm SERDES PHY macros gives me a clear set of engineering constraits, and plenty of previous examples to base my design on, lots of papers, and a clear source of funding for my design.
9. PHY cells are perfect for design automation, because there is high likelyhood of parallelization, and different variations. In light of the funding that Jochen has, I think there's a good likelyhood that I can use funding from this (as PCELLs were one of the goals)

### Next Steps:

* figure out what the PHY infrastructure looks like on DepFET, RD53B, Belle PXD II, LFMonopix/TJMonopix/Malta
* on the above, also figure out what the data rate generation is, and how that 