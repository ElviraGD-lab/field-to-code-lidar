---
title: "Lesson 1.1. Who needs forest growth rates, and why"
---

## Where an engineer starts

Before we write any code or download any data, we work out exactly what needs doing and for whom. We know this from surveying.

Let's think back to how an order for a topographic survey comes in. The client doesn't say 'survey something'. They say something like this:

- we need a plan of the plot for building a house;
- at such-and-such a scale and such-and-such an accuracy;
- the plan must show the relief, the boundaries and the utilities;
- delivered in such-and-such a format.

That's the specification. Without it, we can do a beautiful survey that's no use to the client: the wrong scale, the wrong accuracy, the objects they needed missing. The work is done, but it's no help to anyone.

It's the same with a tool. Today we write the specification for `canopy-growth`. We'll answer five questions:

1. Who is the user?
2. What's their question?
3. What do we have as input?
4. What do we give as output?
5. What don't we promise?

Let's take them in order.

## Step 1. Who is the user

**The tool has several users, and they have different questions.**

- **The distribution company** – for example Viesgo or EDP Redes, which supply the villages around Pajares. It needs to plan forest clearance near the lines: where and when to cut, so that the trees don't reach the wires.
- **Forestry services and researchers.** They need to know how fast forest grows in the mountains: which species, on which slopes, at what altitude.
- **Our own project.** This is the user that's easy to forget. Parts 2 and 3 of Pajares will take data from `canopy-growth`. The risk model needs to know how tall the trees are now and how tall they'll be in a few years.

Why is it important to list them all? Because the user decides what the result has to look like. The company needs a map along the lines. The forestry service needs figures by species and slope. The risk model needs numbers it can plug into a calculation.

✎ **Your data:** the users for our own area (item 1 in the list at the end of the lesson).

## Step 2. What is the user's question

Let's take the main external user, the company. Its question is this: **in how many years will the trees near the line become dangerous?**

Why isn't it enough just to know how tall the trees are today? Let's look at a simple example.

A beech 18 m tall stands 20 m from a wire. Will it fall on the wire? No: even if it falls straight towards the line, it's two metres short. Today it isn't dangerous.

But the beech is growing. According to the forest inventory, the average beech in Asturias adds about 0.1 m a year.[^1] So it will make up the missing 2 m in about 20 years. In 20 years' time, the same beech may well reach the wire.

Height tells us what's dangerous today. Growth rate tells us what will be dangerous tomorrow. Clearance is planned years ahead, so the company needs both.

In the cleared corridor under the line, the picture is different. There the forest was cut down and is growing back: young trees, plenty of light. We expect the forest there to grow noticeably faster than the old beech on the slope. That's exactly what we're going to check.

✎ **Your data:** a tree of our own, its height, its distance from the wire and the growth rate of its species (item 2).

## Step 3. What we have as input

**Our input is the free LiDAR surveys that Spain carries out for the whole country.** The programme is called PNOA.

**What LiDAR is, in short.** We'll go into it properly in the next chapter; for now we only need the gist. Let's picture a reflectorless total station. It sends out a beam and measures the distance to a point from the reflection. Now let's put that instrument on a plane and make it measure hundreds of thousands of points a second. The plane flies over the forest, and we get millions of points with coordinates and height. Some of the points bounce back from the treetops, and some get through the leaves to the ground. A set of points like this is called a point cloud.

From the cloud we can find two surfaces: the ground and the top of the forest. The difference between them is the height of the forest at each spot. The top of the forest is called the canopy.

Spain has already surveyed the whole country with LiDAR several times, in different years. If we compare canopy height in two surveys and divide the difference by the number of years between them, we get the growth rate. For example: it was 17 m, and 12 years later it's 18.2 m. So the forest has grown 1.2 m in 12 years, which means it grew 0.1 m a year.

✎ **Your data:** which LiDAR surveys cover our area, and in which years (item 3).

## Step 4. What we give as output

At this step, every decision has a reason.

- **A growth rate map, in 20–30 m squares, not by individual trees.** The older PNOA surveys have few points: the first survey has half a point per square metre.[^2] We can't reliably make out a single crown from that. But in a 20–30 m square there are already enough points to find the highest spot with confidence.
- **Every number comes with an interval.** It's the same as accuracy in surveying. We'd never hand over a spot height without its error. We don't write '0.3 m a year', we write '0.3 m a year, plus or minus so much'. We covered how to calculate the interval in the statistics book. What matters here is the idea: a number without an error margin is no use to an engineer.
- **A strip passport.** The plane doesn't survey the whole country in one go. It flies in strips, back and forth, like mowing a lawn. The strips are flown on different days, sometimes with different instruments. For each strip we record whether it can be trusted and, if not, why.
- **A report on where the figures can be trusted and where they can't.** If the survey somewhere doesn't let us measure growth, we say so. That's a result too.
- **How to use it.** A Python package for those who write code. A QGIS plugin for those who don't: draw round the area, press a button, get the map.

✎ **Your data:** what accuracy the user needs, and in what form they'd like to get the result (item 4).

## Step 5. What we don't promise

Engineers often skip this step, and they shouldn't. If we don't set out the limits honestly, the user will decide for themselves that the tool can do more than it can, and they'll get it wrong.

- **The growth of an individual tree.** We've already seen why: the older surveys have too few points.
- **A replacement for the company's own flights.** Companies order their own flights over their lines, and those have dozens of times more points: 15–25 per square metre.[^3] We're not competing with that. We give what their flights don't have: the past. Free PNOA surveys going back many years exist for anywhere.
- **The cause of the growth.** We measure how fast the forest grows, but we don't say why: climate, species, soil or something else.
- **A forecast beyond simply extending the trend.** We can say: 'if the forest keeps growing the same way, in 20 years it will be this tall'. But we can't promise that it will keep growing the same way.

✎ **Your data:** what's outside the task for our own area (item 5).

## What we've got

Let's put it all together. Here's our specification in one paragraph:

> `canopy-growth` measures how fast forest height grows, using the free repeat LiDAR surveys from PNOA. The result is a growth rate map in 20–30 m squares with an error margin, a strip passport and a report on where the figures can be trusted. For companies, it shows where and when the forest near lines will become dangerous; for forestry services, how the forest grows; for parts 2 and 3 of the project, how tall the trees are now and will be in a few years. The tool doesn't measure individual trees, doesn't replace the companies' flights, doesn't explain the cause of growth and doesn't promise that the growth will continue.

## ✎ Your data: what we need for the specification

We can have the same conversation for any area. For our own specification, we need to know:

1. **The area and its users.** Where the area is; who needs the result (a company, a forestry service, researchers, our own project) and why each of them needs it.
2. **The main user's question, and an example.** What exactly they want to know. For the example we need one tree near a line: its height, its distance from the wire and the growth rate of its species.
3. **Input data.** Which repeat LiDAR surveys cover the area, and in which years. For Spain, these are the PNOA coverages.
4. **Output.** What accuracy the user needs, what square size suits them, and in what form to hand over the result: a map, a table, a plugin.
5. **Limits.** What the tool doesn't promise for this area, and why.

## Lesson summary

- **Specification** – an agreement on what we're doing, for whom, and what we're not doing. Example: an order for a topographic survey with a scale and an accuracy.
- **Point cloud** – millions of points with coordinates, captured by LiDAR from a plane. Example: points on the treetops and on the ground beneath them.
- **Canopy** – the top of the forest, the surface across the tops of the crowns. Example: a canopy height of 18 m means the top of the forest is 18 m above the ground.
- **Growth rate** – how many metres a year canopy height increases. Example: 1.2 m in 12 years is 0.1 m a year.
- **Strip passport** – a record for each survey strip: whether it can be trusted, and why. Example: a strip was flown leaf-off, so we correct it or exclude it.

## Questions to check understanding

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
## Memory exercises

1. **Recall.** What do we call the top of the forest, the surface across the tops of the crowns?

2. **Fix the mistake.** 'Growth rate = canopy height in the second survey divided by the number of years between the surveys.'

3. **From the prologue.** What brought trees down onto the lines in Asturias in 2018, and how is it different from the south of Spain?

[Answers to these exercises](#en-sol-1-1)

---

## Notes and sources

Sources checked on 19 November 2026.

[^1]: [Fourth National Forest Inventory (IFN4), Asturias: table 926, mean annual height increment by species; beech, 0.11 m a year](https://www.asturias.es/documents/4911212/4919385/20240117-IFN4-indice-tablas-mapas.pdf/ee7edde7-e40f-7331-a482-58a1b76ba5ee).
[^2]: [IGN and MITECO, PNOA-LiDAR project, 2022: 0.5 points/m² in the first coverage](https://www.miteco.gob.es/content/dam/miteco/es/ceneam/grupos-de-trabajo-y-seminarios/patrimonio-natural/20220426_fernandez_guillermo_proyecto_pnoa_lidar_miteco_ign_tcm30-540463.pdf).
[^3]: [5th Smart Grids Congress, 2019: LiDAR flights for power lines usually specify 15–25 points/m²](https://smartgridsinfo.es/comunicaciones/comunicacion-aplicacion-avances-lidar-aerotransportado-modelos-crecimiento-vegetacion-inspeccion-mantenimiento-lineas-transporte).
