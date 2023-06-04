# Prior Work: Tech
- 2007-2011: 130nm, 2012-Present: 65nm
- 28nm available now, but:
	- 2-4x transistors -> longer simulation, layout, verification
	- 3x PDK/DRC rules
	- 2x cost (8k EUR/mm^2)
- Choice depends: A vs D performance, power, area, cost
- ISSCC stats reflect this

# Prior Work: PLL Designs
![[IMG_1496.png]]

![[IMG_1495.jpeg]]

65nm,1.2V±10%, Rad-Hard,

|Design|Fin(Hz)|Fout(Hz)|Jitter(s)|Power(W)|TID(Rad)
|---|---|---|---|---|
|DHP|80M|1.6G,800M,320M|20p||10-20M|
|RD53||1.28G|20ps|||

# Generators: What & Why
- Common analog 'IP' (IBias, VRef, PLL, IO, ADC, DAC) 
- Portable and/or parallel design (65nm or 28nm?)
- Record design method/intent (Why this W/L?)
- Faster modification (e.g. layout ECOs)
- General-purpose tooling (Python, C++, YAML)

## Example Workflow
```mermaid
graph TD

A[("Design Specs
(.yaml)")]-->B("Schematic Generator (.py)")
C[("Circuit Template
i.e. Unsized Netlist
(.cir | .oa)")]-->B
D[("Device Params.
(.yaml)")]-->B
B-->E[("Sized Netlist
(.cir | .oa)")]

W[("Placement Template
(.yaml)")]-->Y("Layout Generator (.py)")
E-->Y
X[("Routing Grid
(.yaml)")]-->Y
Y-->V[("Layout
(.gdsii | .oa)")]
```

# Generators: Procedural

- "White Box" mechanistic modeling & optimization
- Capture known solution to known problem
- Limited simulation for parameters -> Fast
- Top-to-bottom: 'feedforward'

![[IMG_1500.jpeg]]
* Rare 'procedural generator' specimen, circa 2013*

# Generators: Synthesis 

"Black Box" optimization

1. Produce a set of candidates
2. Evaluate via simulation -> Slow!
3. Retain best performing
4. Iterate if necessary: 'feedback'

More formally: *Metaheuristic optimization*

![[IMG_1501.png]]
[https://en.m.wikipedia.org/wiki/Metaheuristic](https://en.m.wikipedia.org/wiki/Metaheuristic)

# Generators: When to use which type?

Linear vs non-linear: aligns with structural vs functional
System vs device
structural vs functional (regular vs non-regular): This is more common in 28nm!!
Schematic vs layout

```mermaid
quadrantChart
    title Reach and engagement of campaigns
    x-axis Low Reach --> High Reach
    y-axis Low Engagement --> High Engagement
    quadrant-1 We should expand
    quadrant-2 Need to promote
    quadrant-3 Re-evaluate
    quadrant-4 May be improved
    Campaign A: [0.3, 0.6]
    Campaign B: [0.45, 0.23]
    Campaign C: [0.57, 0.69]
    Campaign D: [0.78, 0.34]
    Campaign E: [0.40, 0.34]
    Campaign F: [0.35, 0.78]
```

# Generators: Rules of Thumb
- **DO** create a deterministic generator (e.g. avoid random optimization convergence)
- **DO** use constraints (specs, schem/layout templates, routing grids, abstract PDK/DRC)
- **DO** work in GP languages: flexibility, shared w/ real-world testing, readability, source control, sharing w/o NDA
- **DO** combine the two types (What designers already do intutitive)
- **DON'T** hide method in opaque neural networks (human or machine)
	- Overconstrained procedural not reusable and ignores useful abstraction (e.g. drawing raw GDSII)
	- Underconstrained statistical approach time-consuming and meaningless, (e.g. unsupervised learning
- **DON'T** use for unique blocks or top-level
- **DON'T** expect SOTA performance, power, area

# Generators: Survey of Tools

* **PCell & PyCell**: W&L -> OA Layout+BSIM6, SKILL or Python/OA
- BAG: OA Schem Template -> OA Schem, Python+SKILL
- Hdl21 / Layout21

* **gdstk(prev. gdspy)**: Python -> GDSII, Python
- MAGICAL: 
- ALIGN: Netlist -> GDSII, Python, FOSS [1](https://github.com/ALIGN-analoglayout/ALIGN-public)
- Anagen - ????? Closed source, ) [1](https://m.youtube.com/watch?v=IzJbVG-FHJc)
- gdstk

## Potential Issues
- OpenAccess & Cadence
- Environment setup
- Alternate abstraction to learn (pro & con)

# Generators: Example Application

![PLL Diagram]

- PFD:  -->
- Charge Pump:
- Filter: 
- VCO: 
- Divider: Straighforward

![[IMG_1502 1.jpeg]]

[B. Razavi, Design of CMOS Phase-Locked Loops](https://doi.org/10.1017/9781108626200)


