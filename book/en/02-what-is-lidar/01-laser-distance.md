---
title: "Lesson 2.1. How a laser measures distance"
---

## Where we left off

In the first chapter we decided what we're building, and we went through the eight checks without which our number can't be trusted. Now it's time to get to grips with the instrument itself. Let's start with the simplest question of all: how does LiDAR actually measure distance?

## What we want to know

**In this lesson we work out by hand how a laser pulse turns into a distance, and how precise the timing has to be.**

First, let's remind ourselves of what we already know. A reflectorless total station sends a beam at a wall or a tree, catches the reflection and shows the distance. Many of these instruments measure it by time: how long the beam took to get there and back.

Our hunch: LiDAR on a plane does the same thing, just very fast and a great many times over. If the hunch is right, we'll be able to calculate the distance ourselves from the pulse's time of flight. And we'll also be able to estimate how precisely the instrument has to measure time.

## Step 1. How fast light travels

In a vacuum, light travels at 299,792,458 m per second. This number is exact: today the metre itself is defined by it.[^1]

It's handier for us to remember it like this: **in one nanosecond, light travels about 30 cm.** A nanosecond is a billionth of a second.

From here on, we'll work everything out using this number.

## Step 2. There and back

LiDAR sends a short pulse of light downwards. The pulse reaches the ground or a tree crown, is reflected and comes back to the instrument. The instrument measures the time between firing the pulse and its return.

An important detail: the light has covered the distance **twice**, there and back. So the distance is half the path:

**distance = speed of light × time ÷ 2**

Let's work through an example. Say the plane is flying about 1,000 m above the ground. The pulse comes back after 6.67 microseconds. A microsecond is a millionth of a second, so 6.67 µs = 0.00000667 s.

299,792,458 × 0.00000667 ÷ 2 ≈ 999.8 m.

That's almost exactly 1,000 m. The hunch works.

We can do the same calculation in Python. If Python isn't installed yet, we can work these lines out on a calculator instead: we'll go through the installation in a lesson of its own.

First, let's write down the speed of light. Python ignores underscores inside a number; they're only there to make it easier to read:

```python
# Speed of light in vacuum, m/s (exact, SI)
c = 299_792_458
```

Now the time the instrument measured. Writing `6.67e-6` means 6.67 × 10⁻⁶ seconds, that is, 6.67 microseconds:

```python
# Time between sending the pulse and receiving the echo, s
t = 6.67e-6
```

And the formula itself. We divide by 2 because the pulse went there and back. `round(..., 1)` rounds to one decimal place:

```python
# The pulse travels to the ground and back, so we halve the path
distance = c * t / 2
print(round(distance, 1))
```

Python prints `999.8`, that is, 999.8 m. That matches our calculation by hand.

✎ *Your data: the flying height over your own site and the time the pulse takes to come back (item 1 in the list at the end of the lesson).*

## Step 3. How precisely we need to measure time

Now let's work it the other way round. We want tree heights to within about 10 cm. What timing precision does that call for?

Let's turn the formula round: time = 2 × distance ÷ speed of light.

2 × 0.10 ÷ 299,792,458 ≈ 0.00000000067 s, that is, **about 0.67 nanoseconds**.

In Python:

```python
# Wanted distance accuracy, m
dd = 0.10
# Time accuracy needed for it, s
dt = 2 * dd / c
# The same in nanoseconds
print(dt * 1e9)
```

Python prints roughly `0.667`.

So the instrument has to catch the moment the pulse returns to better than one nanosecond. In that time, light travels less than 30 cm. That's exactly why LiDAR is a precise but very complicated instrument: it has very fast clocks inside.

## Step 4. Light travels a little slower in air

**In air, light travels slightly slower than in a vacuum.** Surveyors know this well: before we measure, we enter the temperature and pressure into the total station, and it applies the atmospheric correction by itself.

How much slower? We worked it out with the formula published by NIST, the US standards institute.[^2] For a LiDAR laser near the ground, at 15 °C and normal pressure, light travels about 0.027% slower.[^3]

That seems like nothing. But let's see what it means over 1,000 m:

1,000 m × 0.00027 ≈ 0.27 m.

Without the correction, the error would be about 27 cm for every kilometre. That's more than a beech grows in two years. So the correction is always applied when the survey is processed. We won't calculate it ourselves, but we keep in mind that it's there.

## Step 5. How many pulses a second

LiDAR doesn't measure just one point. It fires pulses again and again, hundreds of thousands of times a second. Let's think about whether there's a limit to that.

Simple logic says it's better to send the next pulse once the previous one has come back. Otherwise the instrument might mix up which pulse has reached it. From 1,000 m, a pulse takes 6.67 µs to get there and back. So one second has room for roughly:

1 ÷ 0.00000667 ≈ 150,000 pulses.

Modern instruments can work faster than that, with several pulses in the air at once. But the idea still holds: the higher the plane flies, the longer each pulse takes and the harder it is to measure often.

✎ *Your data: how many pulses a second are possible from the flying height over your own site (item 2).*

## What we found

Let's put the numbers together. For a flight at about 1,000 m:

| What | How much |
|---|---|
| time of flight of the pulse, there and back | 6.67 µs |
| timing precision for 10 cm | about 0.67 ns |
| error without the atmospheric correction | about 27 cm per 1 km |
| pulses a second, if we wait for each one | about 150,000 |

The hunch is confirmed. LiDAR measures distance in the same way as a reflectorless total station: from the time light takes to travel there and back.

**But a distance isn't yet a point.** To get coordinates, we need to know where the plane was at that moment and which way the beam was pointing. The errors there can be bigger than the errors of the rangefinder itself.

## ✎ Your data: what we need for our own calculation

1. **The flying height over the site.** We can find it in the survey description. For PNOA, it's given in the flight documents.
2. **The pulse's time of flight, and the pulse rate.** We work these out with the formulas from this lesson: time = 2 × height ÷ speed of light; pulses per second ≈ 1 ÷ time.

## Summary

### New terms

- **Time of flight** – the time between firing a pulse and the return of its reflection. Example: from a height of 1,000 m, about 6.67 µs.
- **Distance from time of flight** – speed of light × time ÷ 2. We divide by 2 because the light goes there and back. Example: 6.67 µs is about 1,000 m.
- **Nanosecond** – a billionth of a second. In that time, light travels about 30 cm.
- **Atmospheric correction** – a correction for the fact that light travels slower in air than in a vacuum. Example: without it, the error over 1 km is about 27 cm.

## Questions for an engineer

**Why do we divide by 2 in the distance formula?**

:::{dropdown} Answer
The instrument measures the time the light took to reach the ground and come back. That's a path in both directions, and we need the distance one way.
:::

**Why does LiDAR need such precise clocks?**

:::{dropdown} Answer
In one nanosecond, light travels about 30 cm. To measure heights to within about 10 cm, we have to catch the time to better than one nanosecond.
:::

**Why does a small difference in the speed of light in air matter for our project?**

:::{dropdown} Answer
Over 1 km it gives an error of about 27 cm. That's comparable to several years of forest growth, so without the correction we could mistake the error for growth.
:::

(en-ex-2-1)=
## Exercises

1. **Recall.** How do we calculate the distance if we know the pulse's time of flight?

2. **Fix the mistake.** There's a mistake in the code `distance = c * t`. What is it?

3. **From lesson 1.2.** In which places should the difference between two surveys be almost zero, and why do we compare them?

[Answers to these exercises](#en-sol-2-1)

---

## Notes and sources

Sources checked on 24 November 2026.

[^1]: [BIPM, SI base unit: metre – the speed of light in a vacuum is 299,792,458 m/s, an exact value](https://bipm.org/en/si-base-units/metre).
[^2]: [NIST, Engineering Metrology Toolbox, Index of Refraction of Air (Stone, Zimmerman)](https://emtoolbox.nist.gov/wavelength/Documentation.asp).
[^3]: Project calculation using the modified Edlén formula (NIST, note 2): wavelength 1064 nm, 15 °C, 101,325 Pa, humidity 50% – refractive index of air 1.000274; at 8 °C and 899 hPa (about 1,000 m above sea level) – 1.000249.
