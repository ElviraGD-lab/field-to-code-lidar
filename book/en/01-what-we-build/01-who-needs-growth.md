---
title: "Lesson 1.1. Who needs forest growth rates, and why"
---

## Where we start

Before we download anything or write any code, let's work out exactly what we're making and for whom.

We know this from fieldwork. A client never says 'survey something'. They say something like this:

- we need a plan of the plot for a building project;
- at such-and-such a scale and such-and-such an accuracy;
- showing the relief, the boundaries and the utilities;
- delivered in such-and-such a format.

That's the specification. Without it, we can do a good survey that's no use to the client: the wrong scale, the wrong accuracy, the objects they needed missing. The work is done, but it's no help to anyone.

It's the same with a tool. In this lesson we'll write the specification for `canopy-growth`. To do that, we'll answer five questions:

1. Who will use the tool?
2. What does that person want to know?
3. What do we have as input?
4. What will we hand over to them?
5. What don't we promise?

## Who will use it

**Let's think about who actually needs forest growth rates.** We can see three users.

- **The distribution company.** For example Viesgo or EDP Redes, which supply the villages around Pajares. They need to plan where and when to clear forest near the lines.
- **Forestry services and researchers.** They need to know how fast forest grows in the mountains: which species, on which slopes, at what altitude.
- **Our own project.** This one is easy to forget. Parts 2 and 3 of Pajares will take data from `canopy-growth`. The risk model needs to know how tall the trees are now and how tall they'll become.

Why list them all? Because each of them needs a slightly different result. The company needs a map along the lines. The forestry service needs figures by species and slope. The risk model needs numbers it can plug into a calculation.

✎ **Your data:** who the users are for your own area (item 1 in the list at the end of the lesson).

## What the company wants to know

Let's take the main user, the company. Its question is: **in how many years will the trees near the line become dangerous?**

Why isn't it enough for the company to know how tall the trees are today? Let's work it out with a simple example.

A beech 18 m tall stands 20 m from a wire. Will it fall on the wire? No. Even if it falls straight towards the line, it's 2 m short. Today it isn't dangerous.

But the beech is growing. According to the forest inventory, the average beech in Asturias adds about 0.1 m a year.[^1] It will make up the missing 2 m in about 20 years:

2 m ÷ 0.1 m a year = 20 years.

In 20 years' time, the same beech may well reach the wire.

**Height tells us what's dangerous today. Growth rate tells us what will become dangerous later.** Clearance is planned years ahead, so the company needs both.

In the right-of-way corridor under the line, the picture is different. There the forest was cut down and is growing back: the trees are young and there's plenty of light. We expect the forest there to grow noticeably faster than the old beech on the slope. That's exactly what we're going to check in the project.

✎ **Your data:** one tree near a line – its height, its distance from the wire and the growth rate of its species (item 2).

## What we have as input

**Our input is the free LiDAR surveys that Spain carries out for the whole country.** The programme is called PNOA.

We'll go into what LiDAR is properly in the next chapter. For now we only need the gist. Let's think of a reflectorless total station: it sends out a beam and measures the distance to a point from the reflection. Now let's put that instrument on a plane and make it measure hundreds of thousands of points a second. The plane flies over the forest, and we get millions of points with coordinates and height.

Some of the points bounce back from the treetops, and some get through the leaves to the ground. A set of points like this is called a **point cloud**.

From the cloud we can find two surfaces: the ground and the top of the forest. The top of the forest is called the **canopy**. The difference between the canopy and the ground is the height of the forest at each spot.

Spain has already surveyed the whole country with LiDAR several times, in different years. Let's see how we get growth out of that. We compare canopy height in two surveys:

- in the first survey it was 17 m;
- 12 years later it's 18.2 m;
- the difference: 18.2 − 17 = 1.2 m;
- divided by the number of years: 1.2 m ÷ 12 years = 0.1 m a year.

That's the **growth rate**.

✎ **Your data:** which LiDAR surveys cover your area, and in which years (item 3).

## What we'll hand over to the user

Let's go through each decision and why we're making it.

- **A growth rate map in 20–30 m squares, not by individual trees.** The older PNOA surveys have few points: the first survey has half a point per square metre.[^2] We can't reliably make out a single crown from that. But in a 20–30 m square there are already enough points to find the highest spot.
- **Every number comes with an interval.** It's like accuracy in surveying: we'd never hand over a spot height without its error. So we don't write '0.3 m a year', we write '0.3 m a year, plus or minus so much'. How to calculate the interval is covered in the statistics book. What matters to us here is the idea itself: a number without an error margin is no use to an engineer.
- **A passport of the survey strips.** The plane doesn't survey the whole country in one go. It flies in strips, back and forth. The strips are flown on different days, sometimes with different instruments. For each strip we record whether it can be trusted and, if not, why.
- **A report on where the figures can be trusted.** If the surveys somewhere don't let us measure growth, we say so. That's a result too.
- **An easy way to use it.** A Python package for those who write code. A QGIS plugin for those who don't: draw round the area, press a button, get the map.

✎ **Your data:** what accuracy the user needs, and in what form they'd like to get the result (item 4).

## What we don't promise

This step is easy to skip, and we shouldn't skip it. If we don't set out the limits honestly, the user will decide for themselves that the tool can do more than it can, and they'll get it wrong.

- **The growth of an individual tree.** The older surveys have too few points; we've already been through this.
- **A replacement for the company's own flights.** Companies order their own flights over their lines, and those have dozens of times more points: 15–25 per square metre.[^3] We're not competing with that. We give what their flights don't have: the past. Free PNOA surveys from past years exist for anywhere.
- **The cause of the growth.** We measure how fast the forest grows, but we don't say why: climate, species, soil or something else.
- **A forecast beyond simply extending the trend.** We can say: 'if the forest keeps growing the same way, in 20 years it will be this tall'. But we can't promise that it will keep growing the same way.

✎ **Your data:** what's outside the task for your own area (item 5).

## What we've got

Let's put it all together. Here's our specification in one paragraph:

> `canopy-growth` measures how fast forest height grows, using the free repeat LiDAR surveys from PNOA. The result is a growth rate map in 20–30 m squares with an error margin, a passport of the survey strips and a report on where the figures can be trusted. For companies, it shows where and when the forest near lines will become dangerous; for forestry services, how the forest grows; for parts 2 and 3 of the project, how tall the trees are now and will be in a few years. The tool doesn't measure individual trees, doesn't replace the companies' flights, doesn't explain the cause of growth and doesn't promise that the growth will continue.

## ✎ Your data: what we need for our own specification

We can write the same kind of specification for any area. For that we need:

1. **The area and its users.** Where the area is; who needs the result and why each of them needs it.
2. **The main user's question, and an example.** What they want to know; one tree near a line – its height, its distance from the wire and the growth rate of its species.
3. **Input data.** Which repeat LiDAR surveys cover the area, and in which years. For Spain, these are the PNOA coverages.
4. **The result.** What accuracy is needed, what square size suits, and in what form to hand it over: a map, a table, a plugin.
5. **Limits.** What the tool doesn't promise for this area, and why.

## Summary

### New terms

- **Specification** – an agreement on what we're doing, for whom, and what we're not doing. Example: an order for a topographic survey with a scale and an accuracy.
- **Point cloud** – millions of points with coordinates, captured by LiDAR from a plane. Example: points on the treetops and on the ground beneath them.
- **Canopy** – the top of the forest, the surface across the tops of the crowns. Example: a canopy height of 18 m means the top of the forest is 18 m above the ground.
- **Growth rate** – how many metres a year canopy height increases. Example: 1.2 m in 12 years is 0.1 m a year.
- **Strip passport** – a record for each survey strip: whether it can be trusted, and why. Example: a strip was flown leaf-off, so we correct it or leave it out.

## Questions for an engineer

**Why isn't it enough for the company to know how tall the trees are today?**

:::{dropdown} Answer
Height shows the danger now, but clearance is planned years ahead. A tree that can't reach a wire today may reach it in a few years.
:::

**Why do we calculate growth in 20–30 m squares rather than tree by tree?**

:::{dropdown} Answer
The older surveys have too few points to make out a single crown reliably. In a 20–30 m square there are enough points to find the top of the forest.
:::

**Why write down what the tool doesn't promise?**

:::{dropdown} Answer
Otherwise the user will think the tool can do more than it can, and draw the wrong conclusion. Honest limits are part of the result.
:::

(en-ex-1-1)=
## Exercises

1. **Recall.** What do we call the top of the forest, the surface across the tops of the crowns?

2. **Fix the mistake.** 'Growth rate = canopy height in the second survey divided by the number of years between the surveys.'

3. **From the prologue.** What brought trees down onto the lines in Asturias in 2018, and how is that different from Andalusia?

[Answers to these exercises](#en-sol-1-1)

---

## Notes and sources

Sources checked on 20 November 2026.

[^1]: [IFN4, Asturias, table 926: mean annual height growth by species; beech – 0.11 m a year](https://www.asturias.es/documents/4911212/4919385/20240117-IFN4-indice-tablas-mapas.pdf/ee7edde7-e40f-7331-a482-58a1b76ba5ee).
[^2]: [IGN and MITECO, PNOA-LiDAR project, 2022: 0.5 points/m² in the first coverage](https://www.miteco.gob.es/content/dam/miteco/es/ceneam/grupos-de-trabajo-y-seminarios/patrimonio-natural/20220426_fernandez_guillermo_proyecto_pnoa_lidar_miteco_ign_tcm30-540463.pdf).
[^3]: [V Congreso de Smart Grids, 2019: flights over lines are usually ordered at 15–25 points/m²](https://smartgridsinfo.es/comunicaciones/comunicacion-aplicacion-avances-lidar-aerotransportado-modelos-crecimiento-vegetacion-inspeccion-mantenimiento-lineas-transporte).
