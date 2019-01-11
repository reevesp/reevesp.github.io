---
layout: post
title:  "How many Times in a Day do an Analogue Clock's Hands overlap?"
---


# Clock Puzzle

_There's a few puzzles here. I've compiled them into one article. These are all puzzles that I haven't heard before, so this was all good fun. I also got to Google some maths words which I hadn't used since uni._

## Puzzle

```
1) How many times in a day do an analogue clock's hands overlap?
2) If a stopped clock is right twice a day, how often is a reversing clock right? assuming both were last the same at 00:00?
```

### Assumptions

We are not including a second hands. Our clocks only have hour-hands and minute-hands.

# Puzzle 2 Solution

_Note: There may be better worded solutions to this problem online. You've free to Google._

I think puzzle 2) Is easier to start with.

```
If a stopped clock is right twice a day, how often is a reversing clock right? assuming both were last the same at 00:00?
```

"How many times does a clock show the correct time?" is the same question as "How many times does a clock display the same time as a working analogue clock?"

For these puzzles, we can ignore the minute-hand. Given the hour-hand, we can always determine the location of the minute hand. Therefore, if there is a collision of hour hands then we know that there is also a collision of minute hands. (There exists a function that maps hour-hand position to minute-hand position. This function is onto (but not one-to-one! Or bijective!)

For the two clocks, we will deem the working analogue clock to be the "control" clock which is going at `1 hour-distance / hour`. (I am visualizing the hour-hand moving from its position on the clock face at '12' and travelling the distance to the '1'. This distance between 2 numbers on the clock face is what I am calling an hour-distance. _I'm sorry for choosing this terminology._)

The reversing clock to be going at `-1 hour-distance / hour`. (Same speed, but negative velocity).

|Clock Name|San Dimas Time (Control Clock)|Reversing Clock|
|----------|------------------------------|------------------|
|Clock Starting Time|00:00 (Day 0)|00:00 (Day 0)|
|Time after 1 hour|01:00 (Day 0)|23:00 (Day -1)|

Both clocks do one full revolution of the clock-face in 12 hours.

When both clocks have covered 12 hours-distance in total, then the two clocks will have a collision.
Given that they are traveling in opposite directions from the '12' on the clock face, they begin with 12 hours-distance between them.

Given that each clock is covering `1 hour-distance / hour` in opposite directions the relative speed of the two clocks from each other is `2 hour-distance / hour`. (The hour-hands diverge by this distance per hour).
And given that there are 12 hour-distances in one full revolution of the clock face, we can therefore say that the two clocks will meet every `12 hour-distance / (2 hour-distance / hour ) = 6 hours`.

So, in 24 hours (1 day), the two clocks will collide `24 hours in a day / 6 hours per collision = 4 collisions`.
These times would be:

|San Dimas Time (Control Clock)|Reverse-Speed Clock|
|------------------------------|-------------------|
|00:00 (Day 0)|00:00 (Day 0)|
|06:00 (Day 0)|18:00 (Day -1)|
|12:00 (Day 0)|12:00 (Day -1)|
|18:00 (Day 0)|06:00 (Day -1)|

...and then next at midnight of the following day.

## How many Times a Day does a Clock going at 2x Reverse Speed show the Correct Time?

_I think this question was also posted on Slack. Either that or I am starting to dream up extra problems for myself._

In this situation, if both clocks start at midnight, in one hour the control clock will display 1am and the double-reverse-speed clock with display 10pm.

|Clock Name|San Dimas Time (Control Clock)|2x-Reverse-Speed Clock|
|----------|------------------------------|------------------|
|Clock Starting Time|00:00 (Day 0)|00:00 (Day 0)|
|Time after 1 hour|01:00 (Day 0)|22:00 (Day -1)|

The comparing the two clocks' velocities, the control clock's hour-hand is moving at `1 hour-distance / hour`, and the double-reverse-speed clock is moving at `-2 hour-distance / hour`. The clocks diverge by 3 hours every hour.

Therefore the clocks will meet every `12 hour-distance / ( 3 hour-distance / hour ) = 4 hours`.

So, in a 24 hour day, the two clocks will collide `24 hours in a day / 4 hours per collision = 6 collisions`.
These times would be:

|San Dimas Time (Control Clock)|2x-Reverse-Speed Clock|
|------------------------------|----------------------|
|00:00 (Day 0)|00:00 (Day 0)|
|04:00 (Day 0)|16:00 (Day -1)|
|08:00 (Day 0)|08:00 (Day -1)|
|12:00 (Day 0)|00:00 (Day -1)|
|16:00 (Day 0)|16:00 (Day -2)|
|20:00 (Day 0)|08:00 (Day -2)|

...and then next at midnight of the following day.

## How many Times a Day does a Clock going at Triple Speed show the Correct Time?

We are comparing two clocks:

|Clock Name|San Dimas Time (Control Clock)|3x-Speed Clock|
|----------|------------------------------|------------------|
|Clock Starting Time|00:00 (Day 0)|00:00 (Day 0)|
|Time after 1 hour|01:00 (Day 0)|03:00 (Day 0)|

The triple Speed Clock covers +3 hours-distance / hour, however the control-clock catches up by 1 hour-distance every hour.

Therefore the total deviation between the two clocks is `+2 hour-distance / hour`.

For 12 hours-distance of deviation to elapse (i.e. one collision to occur), we need `12 hour-distance / (2 hour-distance / hour ) = 6 hours`

I.e. a collision occurs every 6 hours.

So, in a 24 hour day, the two clocks would collide `24 hours in a day / 6 hours per collision = 4 collisions`

These times would be at:

|San Dimas Time (Control Clock)|3x-Speed Clock|
|------------------------------|--------------|
|00:00 (Day 0)|00:00 (Day 0)|
|06:00 (Day 0)|18:00 (Day 0)|
|12:00 (Day 0)|12:00 (Day 1)|
|18:00 (Day 0)|06:00 (Day 2)|

...and then next at midnight of the following day.

# Formula for Number of Times a Clock displays the Correct Time given its Speed

For clock operating at speed `v` (hour-distance / hour ) compared to a control clock, these clocks will deviate in hours by `v - 1` every hour. Thus, given a 12-hour clock face:

`There will be a collision every 12 / |(v - 1)| hours`

_Yes, if you compare two control clocks with each other then you are dividing by zero. I suppose you could say that they are infinitely colliding with each other._

Then, the number of collisions in a 24-hr period would be:

`24 / (12 / |(v - 1)|)`

This leads to:

`There will be |2(v - 1)| collisions per 24 hour period`

_N.B. Why `1 - v` or `v - 1`? From the control-clock's perspective, a 2x clock gains 1 hr every hour. Hence `v - 1` describes the deviation per hour. From a 2x clock's perspective, it is correct while the control-clock is losing 1hr every hour._

E.g. For a 5x reversing clock, There would be a collision every `12 / |((-5) - 1)| = 2` hours and this leads to `|2((-5) - 1)| = 12` collisions per 24 hour period.

# Puzzle 1 Solution: How many times in a day do an analogue clock's hands overlap?

_Finally, we get to answering the initial puzzle._

We can model the hours-hand as a control clock, and the minute-hand as a 12x speed clock (in 1 hour of time, the minute hand covers 12 hour-distance by doing a full revolution of the clock face).

Plugging in values, we get that there will be a collision every `12 / |(12 - 1)| = 12/11 =~ 1.0909 recurring` hours.

There will be `|2(12 - 1)| = 22` collisions per 24 hour period.