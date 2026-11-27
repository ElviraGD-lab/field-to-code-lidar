---
title: "Lesson 2.2. How a range becomes a coordinate"
---

## Where we left off

In lesson 2.1 we learned how to get a distance from a pulse's time of flight. But a distance isn't yet a point. **To get X, Y and Z, we need to know where the beam left from and which way it went.**

## What we want to know

**In this lesson we work out by hand how a range turns into the coordinates of a point, and how accurate those coordinates are.**

First, let's think back to the field. With a total station, we set up over a station with known coordinates. The instrument is oriented, so we know the direction. We measure the horizontal angle, the zenith angle and the distance – and we calculate the coordinates of the point. That's the direct (forward) problem in surveying.

Our hunch: LiDAR solves the same problem. There's just one difference: the instrument station is flying, rocking and turning. If the hunch is right, we'll be able to calculate one point at Pajares ourselves, estimate what errors the instruments introduce, and compare that with what PNOA promises.

## Step 1. What we need to know at the moment the pulse is fired

In the field we need three things: where the instrument stands, how it's oriented and where the telescope is pointing. In the plane it's the same, except that each of these things is measured by its own instrument.

1. **Where the plane is – GNSS.** An antenna on the roof of the plane, base stations on the ground. After processing, the position is known to about 2 cm in plan and 5 cm in height.[^1]
2. **How the plane is tilted – the inertial measurement unit (IMU).** It measures three angles: **roll** (wing to wing), **pitch** (nose up or down) and **heading** (where the nose points). The plane is rocking all the time, so PNOA requires these angles to be recorded at least 200 times a second.[^4]
3. **Where the beam went – the scanner mirror angle.** The mirror swings the beam across the flight line, left to right and back. Each pulse leaves at its own angle.

There are two more corrections, familiar from the field:

- **The antenna lever arm.** The antenna is on the roof, and the scanner is in the floor. The offset between them is measured once on the ground and allowed for, just like the instrument height over a station.
- **Boresight calibration.** The axes of the IMU and the scanner don't quite line up. This tiny angle is found with calibration flights over known places.

**All of this together is called direct georeferencing: the coordinates of a point come straight from the plane's instruments, without ground control points for every measurement.**

## Step 2. The simplest case: one right-angled triangle

Let's start simple. The plane is flying due north, wings level, with no roll or pitch. The beam goes off to the right, to the east, at an angle of 20° from the vertical.

Let's draw what we get. It's a right-angled triangle:

```{figure} ../../images/ch02-02-beam-triangle.svg
:alt: Right-angled triangle: the 2128.4 m beam leaves the aircraft at 20° from the vertical, drops 2000 m and moves 728 m east
:width: 80%

The beam and its two legs. A teaching example over the middle of the Pajares frame.
```

- **The hypotenuse** is the beam itself. The instrument measured its length: 2128.4 m.
- **The 20° angle** is between the beam and the vertical (plumb line). The scanner mirror gives us this.
- We need the **two legs**: how far the beam went sideways, and how far it dropped.

If we know the hypotenuse and the angle, we can work out each leg straight away; we don't need Pythagoras for that:

- **leg opposite the angle = hypotenuse × sin of the angle** – the sideways shift;
- **leg next to the angle = hypotenuse × cos of the angle** – how far the beam dropped.

These are the same formulas as for a total station: horizontal distance = S × sin z, height difference = S × cos z. The only difference is that we measure the angle not up from the zenith but down from the vertical.

Let's take an example over the middle of the Pajares frame. The plane is at X = 274 000, Y = 4 763 000, at a height of 3300 m. The ground below is at about 1300 m, so we're flying about 2000 m above the ground. The instrument measured a range of 2128.4 m. This is a teaching example, not a real flight. The heights here are above sea level; we'll look at how they differ from GNSS heights in lesson 3.4.

Let's remind ourselves how the axes work in UTM. This is easy to mix up, so let's be very clear:

- **X is the easting: it increases towards the east.**
- **Y is the northing: it increases towards the north.**
- **Z is the height: it increases upwards.**

Now we calculate the legs:

- sideways: 2128.4 × sin 20° = 2128.4 × 0.342 = **728.0 m**;
- down: 2128.4 × cos 20° = 2128.4 × 0.940 = **2000.0 m**.

Then we add the legs to the plane's position:

- **X** = 274 000 + 728.0 = **274 728.0**. The beam went east, that is, along the X axis, so X has increased.
- **Y** = **4 763 000**. Y doesn't change: the beam didn't move north or south.
- **Z** = 3300 − 2000.0 = **1300.0**. The beam dropped 2000 m.

The point landed on the ground at a height of 1300 m, just as we expected.

**Pythagoras comes in handy as a check.** If we've calculated the legs correctly, together they should give us our range:

728² + 2000² = 529 984 + 4 000 000 = 4 529 984, and √4 529 984 ≈ 2128.4 m.

That's exactly what the instrument measured. The calculation checks out.

Now the same in Python. First, the plane's position. Python ignores underscores inside a number: `274_000` is the same number as 274 000, it just makes the zeros easier to count. We could also write `274000`.

```python
import math

# Aircraft position (example), m: easting X, northing Y, height Z
x0, y0, z0 = 274_000, 4_763_000, 3_300
```

Then what the scanner measured: the range and the angle of the beam from the vertical.

```python
# Measured range, m, and scan angle from the vertical, degrees
r = 2128.4
angle = 20
```

Python's `sin` and `cos` functions expect the angle in radians, not degrees. So first we convert the angle:

```python
# Python's sin and cos take radians, so we convert first
a = math.radians(angle)

# The beam goes east, so only X changes; Y stays the same
x = x0 + r * math.sin(a)
z = z0 - r * math.cos(a)
print(round(x, 1), y0, round(z, 1))
```

Python prints `274728.0 4763000 1300.0`. The answer matches our calculation by hand.

And the Pythagoras check: the two legs should give us back the range.

```python
# Check: the two legs must give back the measured range (Pythagoras)
check = math.sqrt((x - x0) ** 2 + (z0 - z) ** 2)
print(round(check, 1))
```

Python prints `2128.4`.

**Who does this calculation in real work.** Every point in the cloud is calculated by the contractor in their own software: they have the plane's trajectory and the IMU angles, which we don't. The PNOA files already contain the finished X, Y, Z. We did one point by hand to understand where the errors come from and how big they are. We trust the survey documents, but we check them, just as in the field: a total station comes with a calibration certificate, but we still take check measurements. For LiDAR, that check is the stable surfaces (lesson 1.2).

✎ *Your data: the flying height over your own site and the scanner's field of view (item 1 in the list at the end of the lesson).*

## Step 3. What a small angle error does

Now for the main question of the lesson. The IMU doesn't measure angles perfectly. For a good unit, the error in roll and pitch is about 0.005°, and in heading about 0.010°.[^1] That sounds like nothing. Let's see what it turns into on the ground.

In the field we know the rule: **an error of 1″ over 1 km shifts a point by about 5 mm.** That's the small-angle rule: shift = distance × angle in radians.

Let's convert: 0.005° is 18″. Our beam is about 2.13 km long. Shift ≈ 2.13 × 18 × 5 mm ≈ **19 cm**.

```python
# Small-angle rule: shift = distance × angle in radians
shift = r * math.radians(0.005)
print(round(shift, 3))
```

Python prints `0.186`, that is, about 19 cm.

Where this shift ends up depends on where the beam is. **Directly under the plane**, when the beam points straight down, a roll error moves the point sideways and the height hardly changes. **At the edge of the strip**, when the beam is slanted, part of the shift goes into height: at an angle of 20°, that's about 6 cm in height and 17 cm in plan.

**The further from the plane and the bigger the scan angle, the more a small angle error spoils the height.**

## Step 4. The error budget for our point

Let's gather all the errors for our point (a 2128 m beam at 20°). Range accuracy for scanners of this class is about 20 mm.[^2] The errors are independent, so we combine them just as we do in the field when estimating accuracy: we square them, add them up and take the square root.

| Source | In height | In plan |
|---|---|---|
| plane position (GNSS) | 5 cm | 2 cm |
| roll | 6 cm | 17 cm |
| pitch | about 0 | 17 cm |
| heading | 0 on flat ground | 13 cm |
| range | 2 cm | 1 cm |
| **total** | **about 8 cm** | **about 28 cm** |

In height, the point comes out more accurate than in plan. But that's on flat ground.

**On a slope, a plan error turns into a height error.** The point lands 28 cm to one side, and there the ground is already higher or lower. On a 30° slope that's about 16 cm in height, and together with the other errors, about 18 cm. On a 5° slope it's only about 2 cm.

## What we found

| What | How much |
|---|---|
| Pajares point (teaching example) | X 274 728.0; Y 4 763 000; Z 1300.0 |
| Pythagoras check | 2128.4 m – matches the range |
| shift from a 0.005° angle error over 2.13 km | about 19 cm |
| height error on flat ground | about 8 cm |
| error in plan | about 28 cm |
| height error on a 30° slope | about 18 cm |

For the first PNOA coverage, the stated accuracy is better than 20 cm in height and about 30 cm in plan.[^3] Our rough budget gives the same order of magnitude.[^6]

**The hunch is confirmed: LiDAR solves the direct problem of surveying, and the biggest error comes not from the rangefinder but from the angles.**

**This leads to an important conclusion for our project.** Pajares is mountain country with steep slopes. On a slope, a plan error in the survey looks like a change in height, and we could mistake it for forest growth. That's why we look for the shift between surveys on flat, stable places, as we decided in lesson 1.2.

## If something goes wrong

**The point has moved a kilometre.** If we write `math.sin(20)` without converting to radians, Python calculates the sine of twenty radians. The point ends up at X = 275 943 instead of 274 728 – 1.2 km off to the side. When we see a result like that, we immediately ask ourselves: is the angle in degrees or in radians?

**The Pythagoras check doesn't add up.** That means one of the legs has been calculated wrongly: most often sin and cos have been swapped. Let's remember: sin gives the leg opposite the angle (sideways), cos gives the leg next to the angle (down).

## ✎ Your data: what we need for our own calculation

1. **The flying height and the scanner's field of view.** These are given in the survey description. The PNOA technical specifications, for example, give a field of view of up to 50° and a range of up to 3000 m.[^5]
2. **The accuracy of the IMU.** The unit's model is sometimes in the flight report; its accuracy is in the manufacturer's datasheet.
3. **The slope of our own site.** We take it from a terrain model: it shows where a plan error will turn into a height error.

## Summary

### New terms

- **Direct georeferencing** – the coordinates of a point from the plane's position (GNSS), its tilt (IMU), the beam angle and the range. Example: the Pajares point X 274 728, Z 1300.
- **Roll, pitch, heading** – the plane's three angles: wing to wing, nose up and down, where the nose points. Example: a roll error of 0.005°.
- **Scan angle** – the angle of the beam from the vertical. Example: at 20°, the beam went 728 m sideways.
- **Legs from an angle** – sideways = range × sin of the angle, down = range × cos of the angle; Pythagoras is the check. Example: 728² + 2000² ≈ 2128.4².
- **Small-angle rule** – shift = distance × angle in radians. Example: 1″ over 1 km is about 5 mm.
- **Error budget** – all the sources of error in one table, combined through their squares. Example: 8 cm in height, 28 cm in plan.

## Questions for an engineer

**Why does the same roll error spoil the height less under the plane than at the edge of the strip?**

:::{dropdown} Answer
Under the plane, the beam goes straight down, and the angle error only moves the point sideways. At the edge, the beam is slanted and longer, and part of the shift goes into height.
:::

**Why is a point's height worse on a steep slope than on flat ground, even with the same instruments?**

:::{dropdown} Answer
The point lands slightly to one side of where it should be, and on a slope the height is different there. 28 cm in plan on a 30° slope gives about 16 cm in height.
:::

**If the contractor has already calculated all the points, why do we check the survey ourselves?**

:::{dropdown} Answer
For the same reason that in the field we check an instrument with control measurements. Angle errors and survey shifts can't be seen in a single point, but they show up on stable places. Without that check, we could mistake an error for forest growth.
:::

(en-ex-2-2)=
## Exercises

1. **Recall.** Apart from the range, what do we need to know at the moment the pulse is fired to get the coordinates of a point?

2. **Fix the mistake.** There's a mistake in the line `x = x0 + r * math.sin(20)`. What is it?

3. **From lesson 1.1.** For the company, how is tree height different from growth rate?

[Answers to these exercises](#en-sol-2-2)

---

## Notes and sources

Sources checked on 25 November 2026.

[^1]: [Trimble Applanix, POS AV: summary of post-processed accuracy (POS AV 510: X, Y – 0.02 m; Z – 0.05 m; roll and pitch – 0.005°; heading – 0.010°)](https://applanix.trimble.com/en/products/hardware/applanix-pos-av).
[^2]: [RIEGL, LMS-Q780, datasheet: range accuracy 20 mm, field of view 60°](https://www.rieglusa.com/wp-content/uploads/lms-q780-datasheet.pdf).
[^3]: [IGN, "MapaLidar", JIIDE 2019: first coverage – 0.5 points/m², height accuracy better than 20 cm (RMSE Z), 30 cm in plan](https://idee.es/resources/presentaciones/JIIDE19/dia_24/AUDITORIO_PRINCIPAL/1.Sesion_1_Nuevos_Desarrollos/2_MapaLidar_JIIDE2019.pdf).
[^4]: [IGN/CNIG, technical specifications for LiDAR flights, Castilla y León SW (2015): IMU – recording at no less than 200 Hz, drift < 0.1° per hour](https://pnoa.ign.es/resources/archivos/EspTec/2015_Especificaciones_VL_CyLSW.pdf).
[^5]: [IGN/CNIG, technical specifications for LiDAR flights (2016): pulse rate no lower than 45 kHz with a field of view of 50° and a range of up to 3000 m](https://pnoa.ign.es/resources/archivos/EspTec/2016_%20Especificaciones_VL_Madrid.pdf).
[^6]: Lesson calculation: the point, the Pythagoras check, the small-angle rule and the error budget for a 2128.4 m beam at 20°, on 5° and 30° slopes.
