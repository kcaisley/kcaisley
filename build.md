# Website Generation

Use the following script, with SSH keys.

pandoc to generate (make sure to git ignore)

```bash
pandoc --cslthing? --citething? --standalone -o index.html index.md 
```

Then delete the section with hyperlinks, and change the urls to have a simple text representation.

scp to copy
```bash
$ rsync -r /Users/kcaisley/export/website/site/ kcaisley_kcaisley@ssh.phx.nearlyfreespeech.net:/home/public/
```


# Slide Generation


You can use YAML blocks to head slides:

```yaml
---
title:
- Writing in Simple format
author:
- Kennedy Caisley
colortheme:
- dove
fonttheme:
- serif
date:
- 26 July, 2023
---
```


# Simple Writing

We want a simple way to write notes, which then can be compiled to reports, papers, webpages, and presentations. Obviously, reports will be more verbose, but if all we have to do is remove content from a simple test file, to go from a paper to a poster, we are already doing well.

This document itself serves as a valid input `.md` markdown file for generating these reports.

# Necessary Packages

We'll need the following packages:

```
sudo dnf install pandoc texlive 
```

# Slide Markdown Format

Next prepare your markdown file. For slides, note that you can use a YAML block at the top to change things like font, color, title slide date, URL link styles, etc. More info on the YAML can be found on [this blog.](https://github.com/alexeygumirov/pandoc-beamer-how-to)

The slide header, by default the largest, will trigger a new slide. The horizontal rules `---` will do this as well.  Heading levels below the trigger will be sub-headers within a slide. A title page is generated automatically from the YAML header. More info [here.](https://ashwinschronicles.github.io/beamer-slides-using-markdown-and-pandoc)

# Slide Generation

The following one-liner can be used to produce a slide deck:

```
pandoc -t beamer writing.md -o writing.pdf
```

# More

Next, I need to figure out:

* Syntax for properly including images, including SVG and PNG
* Making columns. See: https://github.com/alexeygumirov/pandoc-beamer-how-to
* Other formatting for reports, papers, and websites.
  * For papers, need to figure out nice formatting for units and citations.
* Are posters possible? Like resume, probably just better to do in pure latex.

# Bibtex
natbib and biblatex and the frontend libraries in latex macros
bibtex and biber are the backends


[Building with Pandoc](https://pandoc.org/chunkedhtml-demo/9.1-specifying-bibliographic-data.html)

# Conference Papers, Published vs. Unpublished

For a formally published conference paper, use the biblatex entry type inproceedings (which will be mapped to CSL paper-conference).

For an unpublished manuscript, use the biblatex entry type unpublished without an eventtitle field (this entry type will be mapped to CSL manuscript).

For a talk, an unpublished conference paper, or a poster presentation, use the biblatex entry type unpublished with an eventtitle field (this entry type will be mapped to CSL speech). Use the biblatex type field to indicate the type, e.g. “Paper”, or “Poster”. venue and eventdate may be useful too, though eventdate will not be rendered by most CSL styles. Note that venue is for the event’s venue, unlike location which describes the publisher’s location; do not use the latter for an unpublished conference paper.


dpg
jinst
AIDA presentation


# JINST xamples:

R. Ballabriga, M. Campbell, E. Heijne, X. Llopart and L. Tlustos, The Medipix3 prototype, a pixel
readout chip working in single photon counting mode with improved spectrometric performance, IEEE
Trans. Nucl. Sci. 54 (2007) 1824.
D. Pennicard and H. Graafsma, Simulated performance of high-Z detectors with Medipix3 readout,
2011 JINST 6 P06007.
R. Ballabriga, M. Campbell and X. Llopart, Asic developments for radiation imaging applications: The
medipix and timepix family, Nucl. Instrum. Meth. A 878 (2018) 10.
R. Ballabriga et al., The Medipix3 Prototype, a Pixel Readout Chip Working in Single Photon Counting
Mode With Improved Spectrometric Performance, IEEE Trans. Nucl. Sci. 54 (2007) 1824.

# NIMA examples:
A. Faruqi, G. McMullan, Nucl. Instrum. Methods Phys. Res. A (2017).
I. Dourki, et al., J. Instrum. 12 (03) (2017) C03047.
L. Andricek, et al., IEEE Trans. Nucl. Sci. 51 (3) (2004) 1117–1120.
H.-G. Moser (DEPFET), Nucl. Instrum. Methods Phys. Res. A A831 (2016) 85–87.

D. Jannisa, C. Hofer, C. Gao, X. Xie, A. Beche, T.J. Pennycook, J. Verbeeck,
Event driven 4D STEM acquisition with a Timepix3 detector: microsecond dwell
time and faster scans for high precision and low dose applications, 2021, URL
arXiv:2107.02864v1.

T. Loeliger, C. Broennimann, T. Donath, M. Schneebeli, R. Schnyder, P. Trueb,
The new PILATUS3 ASIC with instant retrigger capability, in: 2012 IEEE Nuclear
Science Symposium and Medical Imaging Conference Record (NSS/MIC) N6-2,
2012, pp. 610–615.

G. Anelli, et al., Radiation tolerant VLSI circuits in standard deep submicron
CMOS technologies for the LHC experiments: Practical design aspects, IEEE
Trans. Nucl. Sci. 46 (6) (1999) 1690–1696.

P. Zambon, V. Radicci, M. Rissi, C. Broennimann, A fitting model of the pixel
response to monochromatic X-rays in photon counting detectors, Nucl. Instrum.
Methods Phys. Res. A 905 (2018) 188–192.

P. Zambon, P. Trueb, M. Rissi, C. Broennimann, A wide energy range calibration
algorithm for X-ray photon counting pixel detectors using high-Z sensor material,
Nucl. Instrum. Methods Phys. Res. A 925 (2019) 164–171.


# JSSC examples:
I. Periá et al., “A high-voltage pixel sensor for the ATLAS
upgrade,” Nucl. Instrum. Methods Phys. Res. A, Accel. Spectrom.
Detect. Assoc. Equip., vol. 924, pp. 99–103, Apr. 2019, doi: 10.1016/
j.nima.2018.06.060.

E. Cavallaro et al., “Studies of irradiated AMS H35 CMOS detectors
for the ATLAS tracker upgrade,” J. Instrum., vol. 12, no. 1, Jan. 2017,
Art. no. C01074, doi: 10.1088/1748-0221/12/01/C01074.

G. Anelli et al., “Radiation tolerant VLSI circuits in standard deep
submicron CMOS technologies for the LHC experiments: Practical
design aspects,” IEEE Trans. Nucl. Sci., vol. 46, no. 6, pp. 1690–1696,
Sep. 1999, doi: 10.1109/23.819140.


[Actual list of BIbtex fields/format](https://en.wikipedia.org/wiki/BibTeX)

# [Printing by type sections](https://www.overleaf.com/learn/latex/Bibliography_management_in_LaTeX):
```
\printbibliography[type=article,title={Articles only}]
\printbibliography[type=book,title={Books only}]

\printbibliography[keyword={physics},title={Physics-related only}]
\printbibliography[keyword={latex},title={\LaTeX-related only}]
```

