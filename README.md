CS 51 website (2012 edition)
================================

What is this?
-------------
This repository contains the source for the website for the Brown University
Computer Science course CS 51: "Models of Computation".

This document provides instructions on how to build and maintain the website.
It is of primary interest to those who wish to, or have no choice but, build and
maintain the website.

Contents
--------
- `Cakefile`  
  automates most tasks related to building the website
- `build`  
  does not exist, but will be created with the website content when you build
- `data`  
  not, strictly speaking, part of the website, but used for generating website
  contents
- `static`  
  the static files that will be part of the website, with no modification
- `style`  
  styles for the site, compiled to CSS as part of the build process
- `templates`  
  page templates in preprocessed form
- `var`  
  website content that is most likely to change,
  drawn upon by the templates in generating the pages

Technology and Setup
--------------------
The templates are written in [Jade][] and the stylesheets are writen in [Less][].
The build file (Cakefile) is written in [CoffeeScript][] and also uses the
[node csv parser][] for processing raw data.

To get these, first install [node.js][], then install everything else by running
`npm install`.

Note: if you don't have CoffeeScript installed globally — which will be the case
if you follow the instructions above — you may want to do the following:

1. Symlink the Cake executable to your main directory (`ln -s node_modules/coffee-script/bin/cake`)
2. Whenever you see `cake` in the instructions below, use `./cake` instead.


[Jade]: http://jade-lang.com/
[Less]: http://lesscss.org/
[CoffeeScript]: http://coffeescript.org/
[node csv parser]: https://github.com/wdavidw/node-csv-parser/
[node.js]: http://nodejs.org/

Building
--------

    cake json
    cake all
    
    cd build
    echo Voilà!

Maintaining
-----------
As mentioned above, the `var` directory contains the content most likely to
change over the course of the class. Go there and modify the file that sounds
like whatever it is you're trying to change.

If you can't find what you're looking for there, you'll need to modify the
templates directly.

If you want to add new static files (e.g., assignment PDFs), put them in the
`static` directory. Everything there gets copied over to the built website.

When you're done, run `cake all` to rebuild the site.

### Modifying assignments and lectures
As you can tell from looking in the `var` directory, assignments and lectures
are stored in JSON files. This makes it really easy to include them in the page
templates, but kind of hard to modify, especially if you're changing a bunch of
them at once.

Instead, you may wish to modify the CSV files in the `data` directory.
(pro-tip: shared spreadsheets in Google Docs, then export as CSV)
Then, run `cake json` to generate the JSON files.  
Warning: this resets the "released" and "solution" flags on the documents.
