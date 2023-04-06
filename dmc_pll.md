## Plan

I want to eventually build a circuit generator, in 65nm and 28nm for the front-end of a detector chip. As a first step, however, I want to at least know how to do this for a PLL. And so I'm going to follow my top down design methodology, and start by building a high level Verilog-A representation of the circuit.





## CDR53

The RD53 chips needed speeds from 40 MHz to 1.28 GHz, and so this is synthesized on chip using just the embedded clock in the

#### Libraries

Important libaries to be used moving forward:

`RD53CDRBONN` is repo for design files of the CDR53B which didn't go into sos (digital flow, verilog modelling) plus PCB design plus whole daq system (hw + fw + sw)

`cdr53bdaq` is a simplified version of RD53_CDR_BONN daq used by external groups for their mesurements e.g. checking link quality along the full data chain. The wiki here has a lot of details on the CDR53B chip from users perspective e.g. explanation of all settings, proper usage, etc.

`CDR53B` contains main library for CDR prototype

`CDR53B_pr` contains Piotrs test benches (no need to check them in)

`CDR_test_km` verification library with top level of testing PLL, alongside logs of verilog-A cells.

`CDR_TEST_CHIP` has more verification verilog-A cells

`IPs/RD53_CDR_BONN_LIB` is similar, but slightly updated, and so I should copy from here
`IPs/RD53_SER_CML_Bonn`
`IPs/` all others are just part of the RD53 project

`technology/` has all the standard cells

#### List of Verilog-A Models Built by Kostas:

```
kcaisley@asiclab008:/faust/user/kmoustakas/cadence/TSMC find . -type f -name "*.va"
./CDR_TEST_CHIP/adder/veriloga/veriloga.va
./CDR_TEST_CHIP/OSC_TIMEDOMAIN_EFF/veriloga/veriloga.va
./CDR_TEST_CHIP/NoiseFlickerWhite/veriloga/veriloga.va
./CDR_TEST_CHIP/FD_TIMEDOMAIN_STD/veriloga/veriloga.va
./CDR_TEST_CHIP/PFD_CP_TIMEDOMAIN_EFF_DEL/veriloga/veriloga.va
./CDR_TEST_CHIP/PFD_CP_PHASE_DOMAIN/veriloga/veriloga.va
./CDR_TEST_CHIP/PFD_CP_TIMEDOMAIN_STD_DEL/veriloga_bk/veriloga.va
./CDR_TEST_CHIP/PFD_CP_TIMEDOMAIN_STD_DEL/veriloga/veriloga.va
./CDR_TEST_CHIP/VCO_PI_TIMEDOMAIN_STD_TABULAR/veriloga/veriloga.va
./CDR_TEST_CHIP/LFSR_VERILOGA/veriloga/veriloga.va
./CDR_TEST_CHIP/PFD_CP_TIMEDOMAIN_STD/veriloga/veriloga.va
./CDR_TEST_CHIP/VCO_TIMEDOMAIN_STD/veriloga_bk/veriloga.va
./CDR_TEST_CHIP/VCO_TIMEDOMAIN_STD/veriloga/veriloga.va
./CDR_TEST_CHIP/DIVIDER_QUAD_TIMEDOMAIN_STD/veriloga/veriloga.va
./CDR_TEST_CHIP/adc20b_va_noCLK/veriloga/veriloga.va
./CDR_TEST_CHIP/NoiseFlickerWhite_osc/veriloga/veriloga.va
./CDR_TEST_CHIP/res_noise_model/veriloga/veriloga.va
./CDR_TEST_CHIP/CP_TIMEDOMAIN_STD/veriloga/veriloga.va
./CDR_TEST_CHIP/VCO_PI_TIMEDOMAIN_STD/veriloga/veriloga.va
./CDR_TEST_CHIP/VCO_TIMEDOMAIN_STD_TABULAR/veriloga/veriloga.va
./CDR_TEST_CHIP/DCD_cell/veriloga/veriloga.va
./CDR_TEST_CHIP/BB_CP_TIMEDOMAIN_STD/veriloga_bk/veriloga.va
./CDR_TEST_CHIP/BB_CP_TIMEDOMAIN_STD/veriloga/veriloga.va
./CDR_TEST_CHIP/NoiseFlickerWhite_v/veriloga/veriloga.va
./CDR_TEST_CHIP/DIVIDER_TIMEDOMAIN_STD/veriloga/veriloga.va
./CDR_TEST_CHIP/BB_CP_TIMEDOMAIN_STD_MOD/veriloga/veriloga.va
./CDR_TEST_CHIP/NoiseFlicker/veriloga/veriloga.va
./CDR_TEST_CHIP/OSC_TIMEDOMAIN_STD_JABS/veriloga/veriloga.va
./CDR_TEST_CHIP/PFD_CP_TIMEDOMAIN_EFF/veriloga/veriloga.va
./CDR_TEST_CHIP/SAVE_BLOCK/veriloga/veriloga.va
./CDR_TEST_CHIP/OSC_TIMEDOMAIN_STD/veriloga/veriloga.va
./CDR_TEST_CHIP/DIVIDER_PHASE_DOMAIN/veriloga/veriloga.va
./CDR_TEST_CHIP/VCO_TIMEDOMAIN_EFF/veriloga/veriloga.va
./CDR_TEST_CHIP/GATED_PD_TIMEDOMAIN_STD/veriloga/veriloga.va
./CDR_TEST_CHIP/delay/veriloga/veriloga.va
./CDR_TEST_CHIP/IradModel/veriloga/veriloga.va
./CDR_TEST_CHIP/CDR_CP_PHASE_DOMAIN/veriloga/veriloga.va
./CDR_TEST_CHIP/res_noise_model_v/veriloga/veriloga.va
./CDR_TEST_CHIP/VCO_PHASE_DOMAIN/veriloga/veriloga.va
./CDR_TEST_CHIP/BB_phasemodel/veriloga/veriloga.va
./CDR_TEST_CHIP/vdelay/veriloga/veriloga.va
./CDR_TEST_CHIP/OSC_TIMEDOMAIN_STD_JABS_DCD/veriloga/veriloga.va
./RD53B_BGP_BGPV_LOCAL/TOP_BGP_BGPV_DUT_ORIGINAL/veriloga/veriloga.va
./RD53B_BGP_BGPV_LOCAL/new_BGR_v1_bk/veriloga/veriloga.va
./RD53B_BGP_BGPV_LOCAL/new_BGR_v1/veriloga/veriloga.va
./VCO_LOOP_SEU_TB/FD_TIMEDOMAIN_STD/veriloga/veriloga.va
./VCO_LOOP_SEU_TB/LFSR_VERILOGA/veriloga/veriloga.va
./VCO_LOOP_SEU_TB/DIVIDER_QUAD_TIMEDOMAIN_STD/veriloga/veriloga.va
./VCO_LOOP_SEU_TB/CP_TIMEDOMAIN_STD/veriloga/veriloga.va
./VCO_LOOP_SEU_TB/BB_CP_TIMEDOMAIN_STD/veriloga/veriloga.va
./CDR_test_km/VCDLorig/veriloga/veriloga.va
./CDR_test_km/PFD_CP_vera/veriloga/veriloga.va
./CDR_test_km/amp/veriloga/veriloga.va
./CDR_test_km/adc20b_va_noCLK/veriloga/veriloga.va
./CDR_test_km/vco_va/veriloga/veriloga.va
./CDR_test_km/dividerjitterquad/veriloga/veriloga.va
./CDR_test_km/dff_rst_va/veriloga/veriloga.va
./CDR_test_km/dig_vco/veriloga/veriloga.va
./CDR_test_km/delay_va/veriloga/veriloga.va
./CDR_test_km/vcojitterquad/verilogabk/veriloga.va
./CDR_test_km/vcojitterquad/verilogaa/veriloga.va
./CDR_test_km/vcojitterquad/veriloga/veriloga.va
./CDR_test_km/vcojitter/veriloga/veriloga.va
./CDR_test_km/jitter_meter/veriloga/veriloga.va
./CDR_test_km/buffer_with_jitter_adjustable_Th_verilogA/veriloga/veriloga.va
./CDR_test_km/VCDL/veriloga/veriloga.va
./CDR_test_km/vco_real/veriloga/veriloga.va
./CDR_test_km/adc20b_va/veriloga/veriloga.va
./CDR_test_km/mux_va/veriloga/veriloga.va
./CDR_test_km/delay/veriloga/veriloga.va
./CDR_test_km/comp_va/veriloga/veriloga.va
./CDR_test_km/dividerjitter/veriloga/veriloga.va
./CDR_test_km/vdelay/veriloga/veriloga.va
./CDR_test_km/dig_pll_lpf/veriloga/veriloga.va
```

#### Pins

VDD: 1.2V (has 10u nmoscap)

VSS: 0.0V

REFCLK: 80MHz

ICP: 10uA for charge pump

IBIAS: 40uA

IBIAS_VCO: 15uA (typical), with 10uA-200uA range ("idc" can be connected directly?)

F1P6G: 1.6GHZ output

F800M:

F320M:

F160M:

FB2FAST:

#### Test CDR53B from Kostas:

<img src="./assets/image-20221028220941970.png" alt="image-20221028220941970"  />



### CDR LIB Block Explanation:

CDS_BLOCK_VERIFICATION_LIB was a testbench to simulate the CDR model with VCD stimuli created by full RD53 chip simulation (separate simulation in digital verification framework). It was used and is working. The purpose is to verify connectivity with rest of chip and check if digital stimuli works as expected. The `CDR_BLOCK` is from `CDR_LIB` which is in RD53 repo
`RD53_DM/RD53B/macros/CDR/cds/CDR_LIB`
this is a so-called CDR macro i.e. it contains all blocks closely related to CDR and placed together - CDR, CMD activity reset, bias DACs, level shifters, phase shifters and data merging





## DMC65

![image-20221028214240457](./assets/image-20221028214240457.png)



3.2 GHz clock is 1/3.2e9 = 3.125e-10  seconds, or 312 ps period.





512 x 512 pix/frame 262 kbit/frame

100 ns /gate

78 kHz frame rate 128 gates



Based on DHPT: depfet hardware handling processing

serializer over gigabit link

you can select multiple frames, and the raw data is dumped into buffer

50x50 or 60x70 um main area, with depfet

all chips are bunch bonded to interposer pcb

current link is at 1.6 gigabits, but we want 3.2 gigabits

DHPT12, design from about 8 years ago
* mostly digital, with a few analog macros
	* PLL, current generation
	* 

* OA library has all the design libraries
* 'utility' has runsets for calibre
* 'verilog' has netlist from Tomek
* cds lib, second line "SOFTINCLUDE path_to_sos/cds.lib
* core is all in digital netlist in Verilog, so we need to import his netlist to make 
* still need bandgap reference


* DMC_Analog has all new parts
	* TSMC padrings are layout-unchanged, but schematics have fixed global into local pins
* FROM -DHPT has old analog parts    

1. Finish 65 tape out with 1.6 Gigabit
2. Create next drop in replacement for 



* Jitter exists in all electrical representations of time
* Becomes significant at GHz speeds, and low signal swing
* We must reduce jitter systematically, by decomposition of the root causes
* An example of a complicated case might be that of a FF that is having it's setup time violated randomly. Is the problems with variations in the clock edge, or in the data edge, though?
* Variations slower than 10Hz are considered 'wander' not jitter
  * Jitter slower than 10Hz is not damaging, as a CDR easily tracks this.
* When a 'digital' clock signal is in transition, voltage amplitude noise is converted into timing jitter
* When analyzing data for jitter, it is easiest to simply use a ideal periodic signal as the timing reference that best fits as the 'minimum sum of squares'.

serializer closer will be faster -> 3.2G
TX/RX with FPGA
rad hard design w/ FF triplication (SEE) vs lifetime effects with corners (Total Ionizing Dose)
bang bang detector
Moscap are modified by radiation, and so usually are avoided
Layout parasitics can cut gain by half

notes from April 19
copy from the RD53 library
macros/pad frame lib/pad frame has example for 10X 

counter inside the chip, only afteer the counter has tripped the threshold, only after is data recoverd

frequency detector iwll not work in CDR mode, as 

four different bias currents, two for the currentp umps, and one for the vco, and once fot eh vco differential to serial converter

charge injection elimination, and voltage follower

rotational frequency divider/detector allows for correction of phase, but not frequency

count reach signal after 4000 succesful input cocks

## PLL Versions Notes:

#### 









Note: **Value Change Dump (VCD)** (also known less commonly as "Variable Change Dump") is an [ASCII](https://en.wikipedia.org/wiki/ASCII)-based format for dumpfiles generated by [EDA](https://en.wikipedia.org/wiki/Electronic_design_automation) [logic simulation](https://en.wikipedia.org/wiki/Logic_simulation) tools. The standard, [four-value](https://en.wikipedia.org/wiki/Verilog#Four-valued_logic) VCD format was defined along with the [Verilog](https://en.wikipedia.org/wiki/Verilog) [hardware description language](https://en.wikipedia.org/wiki/Hardware_description_language) by the [IEEE](https://en.wikipedia.org/wiki/IEEE) Standard [1364-1995](https://en.wikipedia.org/wiki/Verilog#Verilog_1995) in 1996.

From: https://en.wikipedia.org/wiki/Value_change_dump
