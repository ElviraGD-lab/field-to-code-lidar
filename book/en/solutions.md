---
title: "Answers to the exercises"
---

(en-sol-1-1)=
## Lesson 1.1. Who needs forest growth rates, and why

1. The canopy.
2. We don't divide the height, we divide the difference in height: (second height − first height) ÷ number of years.
3. Wet snow on trees that were still in leaf. In Andalusia, it was mostly the wind itself that broke the network.

[Back to the lesson 1.1 exercises](#en-ex-1-1)

(en-sol-1-2)=
## Lesson 1.2. How to prove a number can be trusted

1. A known answer.
2. It's the other way round: we write the threshold down before the calculation, and the commit with the threshold must come earlier in the history than the result.
3. The older surveys have too few points to make out a single crown reliably. In a 20–30 m square there are enough points to find the top of the forest.

[Back to the lesson 1.2 exercises](#en-ex-1-2)

(en-sol-2-1)=
## Lesson 2.1. How a laser measures distance

1. Speed of light × time ÷ 2.
2. It doesn't divide by 2. The pulse goes there and back, so the correct code is `distance = c * t / 2`.
3. On stable surfaces – roads, roofs, rock. They don't grow, so the difference there shows a shift in the survey itself.

[Back to the lesson 2.1 exercises](#en-ex-2-1)

(en-sol-2-2)=
## Lesson 2.2. How a range becomes a coordinate

1. Where the plane is (GNSS), how it's tilted (the IMU: roll, pitch, heading) and at what angle the beam left (the scanner mirror).
2. `math.sin` expects radians. The correct version is `math.sin(math.radians(20))`; otherwise the point moves 1.2 km.
3. Height shows what's dangerous today, while growth rate shows what will become dangerous later.

[Back to the lesson 2.2 exercises](#en-ex-2-2)
