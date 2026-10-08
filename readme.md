# Introduction

This repo contains the source for the end-user manual of Hunter.

# Building the Manual

* Create the HTML files locally:
  * Change directory to `/src`
  * Issue `uv run sphinx-build -b html . build`
  * Change directory up `cd ..`
* Test the HTML files locally:
  * Open the file `build/index.html` in a web browser
