# pyreveal
a python package for converting markdown to reveal.js slides.

## a demo
[![pyreveal demo video](http://wcchin.github.io/images/pyrev_demo_vimeo.png)](https://vimeo.com/226295024)

## Why another converter?
Previously, I was(am) using jupyter to create a notebook with slides, and convert it using jupyter+nbconvert, and even wrote some codes to customize the output html file, and convert to pdf using <a href="https://github.com/astefanutti/decktape" target="blank">phantomjs+decktape</a> automatically. So, the nbconvert can do the conversion from notebook to reveal.js. 

But, I don't understand why they keep the cell tags, which make it a pain while customizing the themes, and the custom.css, that following the tutorials or answers from stackoverflow to customize the reveal.js (the js), the codes will not work for the nbconvert version of slides.html. 

Plus, the speaker notes function is still not working for jupyter+nbconvert. 

One more thing, if you change the contents in the notebook cell, you will need to run the nbconvert command again to generate the static html file. 
What we need, is that the static html file can be generated automatically, and we can see the result as soon as we change the md file.

Therefore, I decided to write a converter that take a simple md, and convert it to reveal.js slides using jinja2. 

## Demo

a demo is <a href="https://wcc-slides.netlify.com/2017/pyreveal/demo.slides.html#/" target="blank">here</a>. 

The codes that generate the demo slides is in the slide_dir directory.

## Install

```sh

git clone https://github.com/wcchin/pyreveal.git
cd pyreveal
pip install -e .

```

## Using

1. cd your_slides_dir
2. create a file named whatever.md (with .md extension)
3. follow the instruction in the following sections and write the slides content inside the whatever.md 
4. run the command, see the following "usage" section
5. if the '-w' is used in the command, the slides will change according to the modification of the whatever.md file (and custom.css). 
6. done

### slides configuration

pyreveal will read the first several lines of the whatever.md file and get the metadata as the configs, using the <a href="https://github.com/waylan/docdata" target="blank">**docdata**</a> package . 

```markdown

title: reveal.js The HTML Presentation Framework
theme: black
transition: concave
cr_word: author - 2017
cr_color: rgba(205,205,205,0.0)
toc: False
to_pdf: false
reveal_path: reveal.js

```

configs:
- title: will be put at the `<title>` part of the `<head>` of the html
- theme: available: black (default), white, league, sky, beige, simple, serif, blood, night, moon, solarized and... (THERE ARE MORE THEME FROM MY THEME DIR)
- transition: none(default), fade, slide, convex, concave, zoom, page
- cr_word: CopyRight word (optional), the words that appear at the footer
- cr_color: color for the cr_word (optional), default to 'rgba(205,205,205,0.0)'
- toc: use the table of content plugin (not tested yet)
- to_pdf: default to false, if true, will create a pdf version of the slides, using phantomjs. to use the to_pdf function, please check and install phantomjs following the <a href="https://github.com/astefanutti/decktape">decktape instruction</a> and <a href="http://phantomjs.org/" target="blank">phantomjs instruction</a>.
- reveal_path: optional, default to 'reveal.js'(the same directory as the output html)

### the special keyword for generating slides and some other functions

pyreveal use a list of escape keyword to generate the function for reveal.js:

```markdown

---keyword

```

where, the *keyword* include: right, down, data*, style*, fragment, notes

### slides break
there are two types of slides break:

```markdown

---right

```

the following slide will appear at right, i.e. a new slide (section).

OR, 

```markdown

---down

```

the following slide will appear at bottom, i.e. a subslide. 

### slide background and style

```markdown

---data*
---style*

```

just same as the reveal.js, they use data-background or something similar to change the per-slide styles.
Therefore, pyreveal will check if there is a ---data-something, then it will put the data-something into and something like the <section data-something> will be generate. 

The same goes for style. 

For more detail, check the corresponding demo.md file that generate something similar to the demo in the original reveal.js website. 

### fragment

for fragment, pyreveal also use something like this:

```markdown

---fragment
this will appear after right or down is press
---fragment_grow
this will grow next

```

pyreveal will just put the tag after the '---' (with replacing the '_' with a blank space) into the <p class="{{ here }}">. So all kind of fragment styles will be supported as they should be in the reveal.js. 

Just some minor different, that shows in the demo file, the word "highlight" does not sit at there at first.

### speaker notes

finally, the speaker notes. 

the speaker notes should be put at the last of each slide, after the tag.
```markdown

---notes
this is the speaker notes
line 2 
line 3

```

### usage

run this and done.

```sh

python -m pyreveal -i whatever.md -w

```

The two arguments:
- -i: take a markdown file that contain the slides content (i.e. what you have prepared using the guidelines). 
- -w: watch the change (optional), and rerun the function if any changes happened, this is very useful for making the slide. press ctrl+c to exit watch. 


### custom.css

Same as the output of the nbconvert, the output of the pyreveal will also try to read a custom.css at the same directory of the output html file. 
So, it is possible and easy to change something for the slides, e.g. change the fonts. 



## Next plan

Next thing to do should be try to read the jupyter notebook file, and convert the slides content from notebook to the reveal.js html. 
Because I still hope that the <cell> tags should be remove, and the speaker notes should be fixed.  


## addition themes
I have added some themes, using the google material design color palette. 
They are:
- material_colorset_dark
- material_colorset_light

The *colorset* include: amber, blue, bluegrey, brown, cyan, deeporange, deeppurple, green, grey, indigo, lightblue, lightgreen, lime, orange, pink, purple, red, teal, yellow. 

e.g.:
- material_amber_dark
- material_blue_dark
- material_green_dark
- material_indigo_dark
- material_grey_light
- material_red_light
- material_purple_light

Please try.
