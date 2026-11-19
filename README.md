# From Field to Code: LiDAR from Scratch

**Del campo al código: LiDAR desde cero**

Learn airborne LiDAR the way an engineer works: start with a real question, read the national surveys of Spain point by point, check every result, and finish with an open tool that measures how fast the forest grows next to power lines.

Book 4 of the *From Field to Code* series. Written in Spanish and English.

> Status: work in progress (started November 2026). Read it online: https://elviragd-lab.github.io/field-to-code-lidar/

## The question

In storms, trees are one of the main causes of power outages, and many of them fall from outside the cleared corridor. To know which trees will reach the wires in a few years, we first need to know how fast the forest grows. Spain has flown the whole country with LiDAR three times (PNOA). Can these free surveys tell us how fast the canopy grows on any site, with an honest error, and where the old surveys cannot tell us anything?

We answer it step by step on the Pajares pass (Asturias and León) while building the open tool [`canopy-growth`](https://github.com/ElviraGD-lab/canopy-growth) ([DOI 10.5281/zenodo.23856842](https://doi.org/10.5281/zenodo.23856842)). No previous knowledge of LiDAR is needed: every idea is explained when it is first used, including the dead ends.

## The series

*From Field to Code* is a series of open books for engineers and field specialists who want to work with spatial data the way the industry does today. Each book follows one real project from the question to a published result.

1. [PyQGIS from Scratch](https://elviragd-lab.github.io/field-to-code-pyqgis/)
2. [Remote Sensing from Scratch](https://elviragd-lab.github.io/field-to-code-remote-sensing/)
3. [Spatial Statistics from Scratch](https://elviragd-lab.github.io/field-to-code-statistics/)
4. LiDAR from Scratch (this book)
5. Power Lines from Scratch (planned)
6. Cloud Native (planned)
7. Storms (planned)
8. Risk (planned)

Series home page: https://elviragd-lab.github.io

## Read and build

The book is written in [MyST Markdown](https://mystmd.org). To build it locally:

```bash
npm install -g mystmd@1.12.0
cd book
myst start
```

## Licence

- Code: Apache License 2.0 (`LICENSE`)
- Text and figures: CC BY 4.0 (`LICENSE-docs`)
- LiDAR data: "PNOA cedido por © Instituto Geográfico Nacional" (scne.es).

© 2026 Elvira Gayanova
