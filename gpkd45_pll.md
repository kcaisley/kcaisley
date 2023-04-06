# 45nm GPDK RAK PLL

I'm trying to figure out how to configure the project.cshrc script located at:

`pll_verification_ws_v1.0_001/WORK/pll_zambezi45/project.cshrc`

I thought originally that I needed to change the first line, but I think the following line is setting the variable to the directory two above the current one:

`setenv PROJECT `cd ../../ ; pwd``

When I run this though, it doesn't work, even in csh, with 'csh -l' option..

I've made some progress, I figured out that I was missing the 'endif' at the end of the bit if statement.

The next thing, is why is this endif statement not evaluating as true? Where is the CDSHOMe defined normally?

The four chips I'm aware the group is working on are:
RD53
Obelix
Cordia
DMC

Additionally, it seems like there might be some work to do with a chip for our local detector

the main 65nm startup script is /kcaisley/cadence/dmc65/run_tsmc65.sh but this just calls /cadence/local/bin/tsmc_crn65lp_1.7a_rd53 pretty much.

I think that the major problem I'm having is that the startup script provided doesn't include everything necessary for starting Cadence.

Let's examine one of the startup scripts for the 65nm process.

### Mon Jun 13
Hans sent me a bunch of instructions, but I'm confused on how he set these up.

at the top level, hans is missing config, readme, and release, 
