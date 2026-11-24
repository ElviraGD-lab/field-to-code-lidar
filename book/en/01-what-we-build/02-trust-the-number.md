---
title: "Lesson 1.2. How to prove a number can be trusted"
---

## Where we left off

In the last lesson we wrote the specification for `canopy-growth`. We decided that the tool produces a map of forest growth rate in 20–30 m squares, and that every number comes with an interval. We also decided to show separately where the figures can be trusted and where they can't.

But that raises a question straight away. Say the tool tells us: 'here the forest is growing 0.3 m a year'. Why should we believe it? And why should anyone who plans clearance work from our map believe it?

## What we want to know

**In this lesson we work out how to check a number that our tool produces.**

First, let's think about how it's done in surveying. We never hand over a survey just because the instrument showed something. We check it:
- we close the traverse and look at the misclosure;
- we check the instrument on a calibration baseline;
- we observe control points whose positions we already know;
- sometimes another surveyor recomputes our field book.

So our hunch is this: we should treat forest growth the same way. One check isn't enough. We need several different ones, and each catches its own kind of error.

And let's write down the main rule right away. **A check must be able to fail.** If a check always passes, whatever we feed into it, it isn't checking anything. So for every check we'll ask: how could it fail?

For part 1 of the project we have eight checks. Let's go through them one at a time.

## Check 1. A known answer

**First we test the method itself on a problem whose answer we already know.**

It's like a calibration baseline for a distance meter. We know the exact distance between two marks, and we see whether the instrument gives the same figure.

With the forest, we do this:
1. We take the most recent and densest survey, from 2025.
2. We 'raise' the trees in it ourselves by a known amount. For example, by 1 m. Now we know exactly what growth should come out.
3. We remove some of the points, so that the survey becomes as sparse as an old one.
4. We run our calculation and see whether it finds that same 1 m, within the error we've stated.

**How it can fail:** the calculation finds not 1 m but, say, 0.6 m. That means there's a mistake in the method itself. We don't go any further until we've found it.

## Check 2. Noise: how much 'growth' comes from errors alone

**We need to know what difference two surveys show even when the forest has hardly grown.**

Over the pass there are two surveys only about a year apart: the Asturian one from 2024 and the León one from 2025. Where they overlap, the forest has barely changed in a year. So almost all the difference between them is survey error, not growth.

For example, we've already compared terrain models from these two surveys on open, flat ground. The typical difference there is about 9 cm.[^1] In the forest it will be larger.

That number is our noise. Growth smaller than the noise we honestly can't tell apart from error. So we publish that kind of growth not as a number but as an upper limit: 'less than so many metres a year'.

**How it can fail:** strictly speaking, this isn't a check but a measurement. But everything else depends on it. If the noise turns out to be large, we simply won't see slow growth, and we'll say so honestly.

## Check 3. Stable surfaces

**Some things don't grow: roads, roofs, rock. On them, the difference between surveys should be almost zero.**

It's like a benchmark. If the height of a benchmark has 'moved' between two levelling runs, it isn't the benchmark's fault, it's our run's.

We compare the surveys at places like these. If the difference there isn't zero, one survey has been shifted in height as a whole. Then we need to remove that shift before we calculate forest growth.

**How it can fail:** on the roads and roofs a noticeable difference remains even after the correction. Then we flag that survey and don't trust it blindly.

## Check 4. The two sides of the pass

**The pass was surveyed by two different programmes: Asturias and León. That gives us two independent series of surveys.**

Where they overlap, they should give the same growth, allowing for the intervals. It's like two independent traverses to the same station: if they agree, we can rest easier.

**How it can fail:** the series give different growth, and the difference is larger than the intervals. Then we look for the cause in the strip passport. For example, one strip may have been flown in autumn, when the leaves were already falling. We publish that analysis too.

## Check 5. Field data

**The forest inventory measures trees on the ground. Its figures are an order-of-magnitude check.**

For example, according to the inventory, the average beech in Asturias grows about 0.1 m a year.[^2] If our tool says that a nearby beech is growing 1 m a year, that's a reason to be wary.

There's one important point here. The inventory measures the average tree, while we measure the top of the canopy. They aren't the same thing. So we don't compare exact numbers, we compare the order of magnitude: are we in the same world?

**How it can fail:** our growth differs from the field figure by more than the intervals allow. Then we publish both figures and explain the difference.

## Check 6. Blind transfer

**We run the same code with the same settings on other sites where we haven't tuned anything.**

These are Galicia and a site with a Mediterranean climate. If the method only works at Pajares, we've quietly tuned it to one place.

**How it can fail:** on the new site the result doesn't pass the checks. If the fault is a bug in the program, we fix it and recalculate Pajares too. If the method simply doesn't work in those conditions, we write down where the limits of its use lie.

## Check 7. Repeatability

**Anyone should be able to run the calculation from scratch and get exactly the same result.**

It's like a field book that another surveyor has recomputed and got the same coordinates.

To compare results, we use a file checksum. It's a short string that a program calculates from the contents of a file, like a fingerprint. If even one digit in the file changes, the fingerprint changes too.

**How it can fail:** when we run it again, the fingerprint is different. That means there's something we haven't recorded somewhere: a different program version, a random selection without a fixed seed, a manual step. That's an engineering mistake, and it has to be fixed.

## Check 8. The threshold comes before the calculation

**We write down the tolerance before the measurement, not after.**

In surveying, the permitted misclosure is taken from the specification in advance. Nobody computes a traverse and then makes up a tolerance that it happens to fit.

It's the same for us. We write the thresholds for all the checks in the `config/acceptance.yaml` file in the `canopy-growth` repository. And we commit the threshold before the result exists. The git history itself shows which came first: the threshold or the number.

Let's look at that file right now. We open the `canopy-growth` repository on GitHub, and in it the `config/acceptance.yaml` file. There's almost nothing inside:

```yaml
thresholds: {}
```

`thresholds` means exactly that: the thresholds. The curly brackets `{}` are an empty list. There are no thresholds yet, because we haven't done a single step of the calculation. The first real threshold will appear before the data download step.

```{figure} ../../images/ch01-02-acceptance-empty.png
:alt: The config/acceptance.yaml file on GitHub, with the rule in the comments and empty thresholds
:width: 100%

The `config/acceptance.yaml` file of the `canopy-growth` repository on GitHub. The lines starting with `#` are comments with the rule: a threshold is written before the step it checks and is not changed after we see the result. Screenshot, 24 November 2026.
```

**How it can fail:** the commit with the threshold appeared after the result, or the threshold was changed after we saw the number. Then the check doesn't count as passed.

## What we found

Let's gather all eight checks into one table.

| Check | What we compare | When it fails | What we do |
|---|---|---|---|
| 1. A known answer | the growth found and the growth we added ourselves | the method doesn't find the known growth | look for the mistake in the method; go no further |
| 2. Noise | two surveys about a year apart | the noise is large | publish slow growth as an upper limit |
| 3. Stable surfaces | roads, roofs and rock in two surveys | a difference remains there | correct for the shift; flag the survey |
| 4. The two sides of the pass | growth from the Asturian and León series | the series disagree | analyse the strips; publish the analysis |
| 5. Field data | our growth and the forest inventory | the difference is larger than the intervals | publish both figures |
| 6. Blind transfer | Galicia and a Mediterranean site | the checks don't pass | fix the bug or state the method's limits |
| 7. Repeatability | file fingerprints from a run from scratch | the fingerprints differ | look for the unrecorded step |
| 8. The threshold comes first | the commit dates of the threshold and the result | the threshold appeared later | the check doesn't count |

**Each check catches its own kind of error, and each one can fail. Only then can the number be handed over to people.**

## ✎ Your data: what we need for the checks on our own site

1. **Two surveys a short time apart.** We need these to estimate the noise. If there aren't any, we estimate the noise from stable surfaces and from the overlap between strips.
2. **Stable surfaces.** Roads, roofs and rock on the site.
3. **Field data on growth.** The forest inventory, or our own tree measurements.
4. **A second site for the transfer.** A similar one or, on the contrary, a completely different one, where we haven't tuned anything.

## Summary

### New terms

- **A check that can fail** – a check that doesn't pass with bad data or a bad method. Example: the method didn't find the growth we added ourselves.
- **Known answer** – a problem whose answer we know in advance. Example: we 'raised' the trees by 1 m and see whether the method finds that metre.
- **Noise** – the difference between surveys that comes from errors alone, with no growth. Example: about 9 cm on open ground between the 2024 and 2025 surveys.
- **Stable surface** – something that doesn't grow or change. Example: a road, a roof, rock.
- **Checksum** – a short fingerprint string for a file. Example: changing one digit in the file changes the fingerprint.
- **Threshold before the calculation** – a tolerance recorded before the result. Example: the commit with the threshold in `acceptance.yaml` comes earlier in the history than the commit with the result.

## Questions for an engineer

**Why is a check that always passes useless?**

:::{dropdown} Answer
It doesn't tell anything apart. A bad method and bad data will pass it too, so it doesn't tell us whether we can trust the number.
:::

**Why do we need two surveys only a year apart, if growth over a year is barely visible?**

:::{dropdown} Answer
That's exactly why. Since the forest has hardly grown, almost all the difference between them is survey error. That's how we find the noise and understand what growth we can see at all.
:::

**Why do we compare our growth with the forest inventory only by order of magnitude?**

:::{dropdown} Answer
The inventory measures the average tree on the ground, while we measure the top of the canopy from above. These are different things, so we can't expect an exact match, but the order of magnitude should be the same.
:::

(en-ex-1-2)=
## Exercises

1. **Recall.** What do we call the check in which we add a known amount of growth to the trees ourselves and see whether the method finds it?

2. **Fix the mistake.** 'We write down the threshold for a check after the calculation, once we can see what result came out.'

3. **From lesson 1.1.** Why do we calculate growth in 20–30 m squares rather than for individual trees?

[Answers to these exercises](#en-sol-1-2)

---

## Notes and sources

Sources checked on 24 November 2026.

[^1]: [Pajares project calculation: 1 m terrain models from the PNOA surveys of 2024 (Asturias) and 2025 (Castile and León), open flat ground; median difference −0.025 m, typical difference (NMAD) 0.089 m. Data: 'PNOA cedido por © Instituto Geográfico Nacional'](https://centrodedescargas.cnig.es).
[^2]: [IFN4, Asturias, table 926: mean annual height growth by species; beech – 0.11 m a year](https://www.asturias.es/documents/4911212/4919385/20240117-IFN4-indice-tablas-mapas.pdf/ee7edde7-e40f-7331-a482-58a1b76ba5ee).
