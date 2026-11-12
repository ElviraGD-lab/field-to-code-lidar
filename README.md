# From Field to Code: LiDAR from Scratch

**Del campo al código: LiDAR desde cero**

Learn airborne LiDAR the way an engineer works: start with a real question, read the national surveys of Spain point by point, check every result, and finish with an open tool that measures how fast the forest grows next to power lines.

Book 4 of the *From Field to Code* series. Written in Spanish and English.

> Status: work in progress (started November 2026).

## The question

Power line corridors are cleared on a fixed cycle, not on the measured growth of the forest. Spain has flown the whole country with LiDAR three times (PNOA). Can these free surveys tell us how fast the canopy grows on any site, with an honest error, and where the old surveys cannot tell us anything?

We answer it step by step on the Pajares pass (Asturias and León) while building the open tool [`canopy-growth`](https://github.com/ElviraGD-lab/canopy-growth). No previous knowledge of LiDAR is needed: every idea is explained when it is first used, including the dead ends.

## The series

*From Field to Code* is a series of open books for engineers and field specialists who want to work with spatial data the way the industry does today. Each book follows one real project from the question to a published result.

1. [PyQGIS from Scratch](https://github.com/ElviraGD-lab/field-to-code-pyqgis)
2. [Remote Sensing from Scratch](https://github.com/ElviraGD-lab/field-to-code-remote-sensing)
3. Spatial Statistics from Scratch (in preparation)
4. LiDAR from Scratch (this book)

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
