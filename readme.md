# Introduction

This repo contains the source for the end-user manual of Hunter.

# Building the Manual

## Pre-requisites

* On a recent Linux distribution, install the following packages:
  * `python3-sphinx`
  * `python3-uv`
* Run `uv venv` and then `uv sync`.


## Generating the Manual as HTML Files

* Create the HTML files locally:
  * Change directory to `/src`
  * Issue `uv run sphinx-build -b html . build`
  * Change directory up `cd ..`
* Test the HTML files locally:
  * Open the file `build/index.html` in a web browser
* Commit and push the changes to GitHub (`main` branch)


## Publishing the Manual to GitHub Pages

Run the GitHub Action `Build and publish manual`.
