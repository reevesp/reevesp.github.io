---
layout: post
title:  "Truck Travel Puzzle"
---

## Truck Travel Puzzle

_I got very distracted and went off on quite a few tangents with this puzzle. I suppose that is part of the fun._

# Puzzle

```
You have 5 trucks. Each truck has a tank of 100 litres of fuel. it also has  a spare tank of 100 litres. A truck can do 10 miles on a litre of fuel.

You need to transport someone along a road just using the trucks. Fuel can be transferred at will between the trucks. How far can you get? How do you do it?
```

That being said, I'm not sure how watertight the wording of this puzzle is. 

## Nitpicks / Assumptions

* I don't see the point of distinguishing between the two tanks, I'm assuming that the entire 200 litre capacity can be transferred. Unless perhaps only the spare tank can be donated to other trucks.

* Do the trucks need the single transported person to be present in order to drive them? i.e. can the trucks move if they are not currently being driven by the person being transported? Can it be assumed that every truck comes with a driver?

* What fuel do we start with? Does each truck start with 100 litres? Definitely. Does each truck start with the spare tank filled with 100 litres for a total of 200 litres? I'm not sure.

* Are there any points on the road where the trucks can replensish themselves or refuel? Is there an infinite fuel source at the start point?

* "Fuel can be transferred at will between the trucks". Even if they are not next to each other? From both the main and spare tanks?

* I assume that the road is a hypothetically smooth road which is infinitely long. It is not a real road. There is no change in gradient and so I cannot roll the trucks down the infinitely long road for free.

## Initial "cheap" answers

1. Reverse the trucks by a very tiny fraction and then claim to have travelled the entire circumference of the earth and come back to your starting point. (the top result on google says that the equatorial circumference of Earth is 24,901 miles).

2. In fact, if you really want to take the mick with the circumference / modular arithmetic joke, feel free to add on however many multiples of the earth's circumference as you want, and claim that you've done a few laps of the planet.

3. If the trucks can truly transfer "teleport" fuel between them then you can send one truck out to burn through the entire fuel supply of all the trucks (with its tank being perpetually topped up from the other trucks via teleport). So, if all the trucks start with only the 100L tank full, the single truck would burn `5 * 100L = 500L` of fuel and thus travel `500L * 10miles = 5000miles`. If all the trucks start with both tanks filled up, then a single truck could travel `5 * 200L * 10miles = 10,000miles`. If there is an infinite petrol fuel source at the starting point, then a single truck could travel infinitely far.

## Why do I keep talking about an infinite fuel source?

I'll admit, I was initially lead down the garden path as I read this puzzle. I was already thinking about Conway's Soldiers (https://en.wikipedia.org/wiki/Conway's_Soldiers) when the puzzle mentioned trying to get as far as possible from a starting point, the crux of the logic being around the interaction between the trucks, and that the trucks can give each other "moves" by being positioned next to each other.

This is a significant diversion, so I have ignored this for now.

# "Trucks start with 200 Litres" Solution

_I found that the scenario where each truck starts with 200L of fuel is simplest._

I came to this solution by working from the simpler solutions for the 1-truck case, 2-truck case, etc.

I'm following the logic that:

* The puzzle will always end with a single truck driving forwards by itself for the final stretch.

* All other trucks are there to get this "endgame" truck as far forward as possible before it starts its solo 200L run.

* We want to waste as little fuel as possible transporting the supporter-trucks further than necessary. As soon as the total fuel of the convoy can be carried by one less truck, that truck should be abandoned. _(I'm actually visualizing the trucks as one big 1000L capacity truck, and whenever the convoy has used 200L of capacity then that could be a truck that is now surplus to requirements. Imagine if we were given 1million trucks but still only 1000L of fuel. You'd instantly discard all but 5 trucks because there is nothing to be gained for taking extra trucks with you!)_

#### 1-Truck Case
When there is one truck, it starts with 200L and it travels `200L * 10miles = 2000 miles`.

#### 2-Truck Case
When there are two trucks, both trucks travel forward until 200L of fuel has been used between them. `200L / 2trucks = 100L` to travel on, before one of the two trucks gives all its fuel to fill the other truck to the brim and then is abandoned. Then, the "endgame" truck can travel for 200L worth of fuel.
Therefore, for 2 trucks:

|Leg|Miles Travelled during Leg|
|---|--------------------------|
|2nd-to-last|`200L / 2 trucks * 10 miles = 1000 miles`|
|last|`200L / 1 truck * 10 miles = 2000 miles`|

Totalling the 100L of distance from the first leg plus the 200L from the second leg gives 3000 miles travelled.

#### 3-Truck Case

For 3 trucks, the third truck is abandoned after the 3 trucks have driven `200L / 3 trucks =~ 66.6l` from the first leg. Then the 2 trucks are at full capacity and the process is the same as the 2 truck case.


Therefore, for 3 trucks:

|Leg|Miles Travelled during Leg|
|---|--------------------------|
|3rd-to-last|`200L / 3 trucks * 10 miles =~ 666 miles`|
|2nd-to-last|`200L / 2 trucks * 10 miles = 1000 miles`|
|last|`200L / 1 truck * 10 miles = 2000 miles`|

This totals to =~ 3666 miles.

#### n-Truck Case

We can write the distance the convoy covers in the `nth-from-last-leg` as `200L / n trucks * 10 miles` or `2000/n miles`.

So we can get the distance elapsed from the convoy with the harmonic series:
`sum from k=1 to n of 2000/k miles`

The 2000 can be factored out to give:

`2000 * (sum from k=1 to n of 1/k) miles`

#### 5-Truck Case

So for 5 trucks, n = 5.

Therefore, `2000 * (sum from k=1 to 5 of 1/k)` => `2000 ( 1 + 1/2 + 1/3 + 1/4 + 1/5 ) =~ 4,566.6 recurring` miles travelled.

# "Trucks start with 100 Litres" Solution

Here is my solution for where the trucks start only half full.

This doesn't boil down to a nice sequence as easily, we need to discard a few trucks at the start. At the start line we are concentrating all of the fuel into a portion of trucks.

#### 5-Truck Case

For the 5-truck case, we start with 500L of fuel. This fills 2 trucks with 200L and half-fills a third truck with 100L. The three legs will be:

Therefore, for 3 trucks

|Leg|Miles Travelled during Leg|
|---|--------------------------|
|3rd-to-last|`100L / 3 trucks * 10 miles =~ 333 miles`|
|2nd-to-last|`200L / 2 trucks * 10 miles = 1000 miles`|
|last|`200L / 1 truck * 10 miles = 2000 miles`|

This totals to =~ 3333 miles.

#### n-Truck Case

_I'm not really that happy about this, perhaps someone in my team will get a better solution than this over the holidays.

Whenever there is an odd number of trucks, there will always be enough fuel to fill `n/2` trucks fully, and an `n/2 + 1`'th truck that starts with 100L of fuel.

Wherever there is an even number of trucks, there will be `n/2` legs.

For even values of n, the total distance driven is:

`2000 * (sum from k=1 to n/2 of 1/k)`

For odd values of n, the total distance driven is:

`2000 * (sum from k=1 to n/2 of 1/k) + 100L / (n/2 + 1) * 10miles`

Which boils down to:

`2000 * (sum from k=1 to n/2 of 1/k) + 2000/(n + 2)`

_I don't think that you really gain anything from factoring out the 2000 here._