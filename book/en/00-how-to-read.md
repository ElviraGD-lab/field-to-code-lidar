---
title: "How to read this book"
---

## What we're doing

**In this book we build an open tool called `canopy-growth`. It measures how fast forest grows, using the free repeat LiDAR surveys that Spain carries out for the whole country.**

We looked at why this matters in the prologue. A tree that can't reach a wire today may reach it in a few years. To know where and when that will happen, we need more than the height of the forest: we need to know how fast it's growing.

We build the tool from scratch, step by step. Right now, the [`canopy-growth`](https://github.com/ElviraGD-lab/canopy-growth) repository on GitHub holds only the foundation: the package, the tests and automatic checks. It can't calculate anything yet. Everything else we'll make together.

## Who this book is for

**For engineers who have worked in the field with an instrument and know their job, but know nothing about LiDAR.**

For example, a surveyor who has measured with a total station and GNSS, drawn up plans and worked in QGIS. We take maps, coordinate systems and QGIS as familiar. LiDAR itself, point clouds and the software for them we explain from the very beginning, at the moment we need them.

The author of this book is learning too. So the book has real difficulties in it, real mistakes, and how we worked through them.

## What we learn

Everything we need to get the project's result, in order:

- **What LiDAR is.** How it measures distance, and how a distance becomes a point with coordinates.
- **The point cloud.** How the file is put together, when the plane flew, and which height system the points are recorded in.
- **The national PNOA surveys.** What Spain has already surveyed, and how to download it and look at it.
- **PDAL.** The main open program for working with point clouds.
- **The ground and forest height.** How to separate the ground from the trees and find the canopy height.
- **Comparing surveys.** How to compare surveys from different years, instruments and seasons, and tell real growth from survey error.
- **The finished tool.** A Python package, tests, publication with a DOI and a QGIS plugin.

## What the other books in the series cover

Each book in the series teaches one thing. What's already explained in the other books we don't repeat here; we simply use it:

- Python and PyQGIS – [book 1, *PyQGIS from Scratch*](https://elviragd-lab.github.io/field-to-code-pyqgis/);
- satellite imagery, light and terrain – [book 2, *Remote Sensing from Scratch*](https://elviragd-lab.github.io/field-to-code-remote-sensing/);
- statistics: median, error, intervals – [book 3, *Spatial Statistics from Scratch*](https://elviragd-lab.github.io/field-to-code-statistics/).

If any of these topics is new to us, it's explained from scratch there.

## What software we need

All the software is free and open. A computer running Windows, macOS or Linux will do.

- **QGIS** – to look at point clouds and maps with our own eyes.
- **Conda** – a program that installs Python and all the libraries we need in a separate environment. That way everyone has exactly the same versions, and the results can be repeated. The list of libraries is in the `environment.yml` file in the `canopy-growth` repository.
- **Python 3.12** with these libraries: PDAL for point clouds, NumPy for calculations, GDAL and rasterio for rasters, geopandas for vector layers, matplotlib for charts and pytest for checking the code.
- **Git and GitHub** – to keep the project's history and publish it.

We'll go through how to install all of this in a lesson of its own.

## How a lesson works

**One lesson, one step of the project.** If a step is hard, the chapter gets more lessons.

Every lesson has:

- **first, the question:** what we want to know and what we think the result should be;
- **then the work, step by step:** the step, the code and an explanation of every line;
- **then the check:** does it match what we expected? If it doesn't, we work out why;
- **✎ Your data:** notes wherever we can put in our own area or our own data, and at the end of the lesson a list of the data we need for it;
- **📸:** a place where the lesson has a screenshot;
- **the summary:** the new terms, what they mean, and an example;
- **questions to check understanding:** the answer comes straight after the question, but we think it through ourselves first;
- **three memory exercises:** the answers are on the ['Answers to the exercises'](./solutions.md) page at the end of the book.

## About the code

**The code, the comments in it and the figure captions are in English.** That way we can use the code straight away, and the figures are the same in both versions of the book.

**All the project's code is in the `canopy-growth` repository.** At the end of each chapter there's a link to a commit, that is, the saved version of the code that the chapter worked with.

**It's better to type the code by hand than to copy it.** That way it sticks faster. If the code doesn't work, we compare it with the book character by character: most often a bracket or a quotation mark is to blame.

## About mistakes

**We don't hide our mistakes.** If a result doesn't match what we expected, that's a result of the project work too. We look at why it happened and what to do next.

**We check every number.** Every step ends with a check that can fail. We write down the threshold for a check before the calculation, not after, so that we can't bend it to fit the result.

## If something doesn't work

Software gets updated, and data on the internet sometimes moves. If code from the book stops working, or there's a mistake in the text, please report it on GitHub, under Issues:

- about the book – in the [`field-to-code-lidar`](https://github.com/ElviraGD-lab/field-to-code-lidar/issues) repository;
- about the tool itself – in the [`canopy-growth`](https://github.com/ElviraGD-lab/canopy-growth/issues) repository. Here it really helps to give the names of the PNOA tiles, the command and the full text of the error.

## Licences

- **The book's code and `canopy-growth`** – Apache-2.0: it can be used and changed in our own work.
- **The book's text and figures** – CC BY 4.0: they can be used with credit to the author.
- **The QGIS plugin** – GPL v2 or later, as QGIS requires.
- **LiDAR data** – 'PNOA cedido por © Instituto Geográfico Nacional', CC BY 4.0. We don't put the data itself in the repository: the code downloads the tiles by itself.
