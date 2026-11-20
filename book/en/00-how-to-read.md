---
title: "How to read this book"
---

## The `canopy-growth` project

**`canopy-growth` is an open tool that measures how fast forest grows, using the free repeat LiDAR surveys from PNOA.** It's the first part of the Pajares project.

**The question it answers.** A tree that can't reach a wire today may reach it in a few years. To know where and when the forest near lines will become dangerous, we need more than the height of the forest: we need to know how fast it's growing. Spain has already surveyed the whole country with LiDAR several times. From these surveys we measure how much the forest has grown, and we're honest about where the older surveys don't let us measure it.

**Who it's for.**

- Distribution companies – to plan clearance work near lines.
- Forestry services and researchers – to know how forest grows in the mountains.
- Parts 2 and 3 of the Pajares project – they need tree heights now and in a few years' time.

**What it gives us.**

- A map of forest height growth rate in metres per year, in 20–30 m squares, with an uncertainty interval.
- A strip passport for each survey strip: usable, corrected or excluded, and why.
- A report on where the figures can be trusted.
- A Python package and a command-line interface for those who write code. A QGIS plugin for those who don't: draw round the area, press a button, and the plugin downloads the PNOA tiles and does the calculation by itself.

**What it doesn't promise.**

- The growth of an individual tree.
- A replacement for the companies' own flights: they have dozens of times more points.
- The cause of the growth.
- A forecast that goes beyond simply extending the measured rate.

**Where it stands now.** The repository is open on GitHub: [ElviraGD-lab/canopy-growth](https://github.com/ElviraGD-lab/canopy-growth). Version 0.1.0 is the foundation of the project: the package, the tests and automatic checks. It can't calculate anything yet. Everything else we build in this book, step by step. The project has a DOI: [10.5281/zenodo.23856842](https://doi.org/10.5281/zenodo.23856842).

## What we learn in this book

We build `canopy-growth` from scratch, and along the way we learn everything we need for it:

- **LiDAR itself.** How a laser measures distance, how a distance becomes a point with coordinates, and what LiDAR sees in a forest.
- **The point cloud and its files.** How LAS and LAZ files are put together, how to find out when the plane flew, and which height system the points are recorded in.
- **The national PNOA surveys.** What Spain has already surveyed, how to download a tile and how to look at it.
- **PDAL** – the main open tool for point clouds.
- **Ground and forest height models.** How to separate the ground from the trees, and how to get canopy height.
- **Comparing surveys from different years.** How to line up surveys made with different instruments, in different years and seasons, and how to tell real growth from survey errors.
- **The tool.** A Python package, tests, automatic checks, a release with a DOI and a QGIS plugin.

**What we don't explain in detail here.** That's already covered in other books in the series. Here we use it and show how we reason:

- Python and PyQGIS – [book 1, *PyQGIS from Scratch*](https://elviragd-lab.github.io/field-to-code-pyqgis/);
- satellite imagery, light and terrain – [book 2, *Remote Sensing from Scratch*](https://elviragd-lab.github.io/field-to-code-remote-sensing/);
- statistics: median, error, intervals – [book 3, *Spatial Statistics from Scratch*](https://elviragd-lab.github.io/field-to-code-statistics/).

## Who this book is for

For engineers and field specialists who know their job but don't know LiDAR. For example, a surveyor who has worked with a total station and GNSS and wants to learn to work with point clouds the way the industry does today.

We don't need to know LiDAR beforehand: we explain each idea at the point where we first need it. Python at the level of book 1 will come in handy.

## What we use

All the software is free and open.

- **QGIS** – to look at point clouds and maps with our own eyes. Version 4.2 LTR, as in book 1. Download it from [qgis.org](https://qgis.org).
- **Conda** – a program that installs Python and all the libraries we need in a separate environment. That way everyone has the same versions, and the results can be repeated. The environment itself is described in the `environment.yml` file in the `canopy-growth` repository.
- **Python 3.12** and the libraries inside the environment:
  - PDAL and python-pdal – to read, filter and process point clouds;
  - NumPy – to calculate over millions of points at once;
  - GDAL and rasterio – rasters: ground and forest height models;
  - geopandas – vector layers: area boundaries, lines;
  - matplotlib – charts and figures;
  - pytest – tests that check the code.
- **Git and GitHub** – to keep the project's history and publish it.

We go through how to install everything in the lesson on the environment. We need a computer running Windows, macOS or Linux, an internet connection and disk space for the LiDAR tiles.

## How the book works

**One chapter, one stage of the project.** The chapters follow the order in which `canopy-growth` is built: first the problem, then LiDAR itself, the files, PDAL, the data, survey strips, the ground, forest height, noise and growth rate, checking in the field, moving to other areas and, finally, the finished tool.

**One lesson, one step.** If a step is hard, the chapter has more lessons.

**How a lesson works.**

- **Work steps:** the step, the code, and an explanation of every line.
- **✎ Your data** – notes wherever we can put in our own area or our own data. At the end of the lesson there's a list of what data we need and what for.
- **📸** – a place where a screenshot goes into the lesson.
- **Lesson summary** – the new ideas: what each one is, with an example.
- **Questions to check understanding** – the answer comes straight after the question. We think it through ourselves first, then check.
- **Memory exercises** – three short exercises to make things stick. The answers are on the ['Answers to the exercises'](./solutions.md) page at the end of the book.

## About the code

**The code, the comments in it and the figure captions are in English.** That way we can put the code straight to work, and the figures are the same in both versions of the book.

**All the project's code lives in the `canopy-growth` repository.** At the end of each chapter there's a link to a commit – a saved version of the code that the chapter works with. So we can open exactly the code being discussed.

**It's better to type the code by hand than to copy it.** That way our hands and heads remember it faster. If the code doesn't work, we check it against the book character by character: most often a bracket or a quotation mark is to blame.

## About mistakes

**We show our mistakes rather than hiding them.** If a result doesn't match what we expected, that's part of the work too. We look at why it happened and what we do next.

**We check every number.** Every step ends with a check that can fail. We write down the thresholds for a check before the calculation, not after.

## Licences

- **The book's code and `canopy-growth`** – Apache-2.0. We can take it, change it and use it in our own work.
- **The book's text and figures** – CC BY 4.0. They can be used with credit to the author.
- **The QGIS plugin** – GPL v2 or later: QGIS requires this.
- **LiDAR data** – 'PNOA cedido por © Instituto Geográfico Nacional', licence CC BY 4.0. We don't put the data in the repository: the code downloads the tiles itself.

## If something doesn't work

Software gets updated, and data on the internet sometimes moves. If code from the book stops working, or there's a mistake in the text, write to us on GitHub, under Issues:

- about the book – the [field-to-code-lidar](https://github.com/ElviraGD-lab/field-to-code-lidar/issues) repository;
- about the tool – the [`canopy-growth`](https://github.com/ElviraGD-lab/canopy-growth/issues) repository. For a bug in the tool, it helps to give the names of the PNOA tiles, the command and the full text of the error.

That way the book and the tool get better for everyone.
