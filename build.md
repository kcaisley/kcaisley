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
