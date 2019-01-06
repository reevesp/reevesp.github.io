---
layout: post
title:  "100 Coins Puzzle"
---

## 100 Coins Puzzle

_Sadly, I've seen this puzzle before. I know that the email said no googling/no research (well, I feel that is the spirit of the email), however I feel the need to declare my prior knowledge. Don't worry, I solved it when I encountered it the first time too._

# Puzzle

```
You have 100 coins. 10 are heads. 90 are tails. you cannot look/feel to see which way up the coins are. How do you divide them into two piles, each with the same number of heads showing in them?
```

## Initial "Cheap" answers.

1. Divide the coins into two equal sets of 50 coins, and then stand all the coins on their edge. Hence, both the heads-side and the tails-side are visible and each group has the same number of heads showing.

2. Divide the coins into two equal sets of 50 coins, and do this on a glass table. Hence, both groups coins have the same number of heads showing.

# Puzzle Solution

We consider the starting set of 100 coins to be pile 1, and pile 2 currently has zero coins.

We need a per-coin function or process that will give a "positive outcome" and is not related to whether the selected coin is currently showing heads or tails.

_When i say "positive outcome" i think of the two hypothetical groups of coins as having a quantity of heads, and the initial group (100coins, 10 heads, 90 tails) as having a `+10 head-advantage` (the difference in number of heads between pile 1 and pile 2)._

Looking at the puzzle setup, we can see that taking a coin from pile 1, flipping it, and adding it to pile 2 satisfies our requirement.

The flip is necessary to make sure that the operation of moving a coin from pile 1 to pile 2 always results in a change of `-1 head-advantage`.

If a tails-coin is selected from pile 1, flipped, and added to pile 2 then pile 1 does not lose a head but pile 2 gains a head. This is a change of `-1 head-advantage` to pile 1.
If a heads-coin is selected from pile 1, flipped and added to pile 2, then pile 1 loses a head while pile 2 does not gain. Again, this is a change of -1 `head-advantage` to pile 1.

For example:

|Operation| Pile 1 Contents (Total Coins/Heads/Tails)| Pile 2 Contents (Total Coins/Heads/Tails)| Head-Advantage (Pile 1 Heads - Pile 2 Heads)|
|---------|------------------------------------------|------------------------------------------|---------------------------------------------|
|Start| 100/10/90| 0/0/0| +10|
|After a Head is processed| 99/9/90| 1/0/1| +9 (-1 change)|
||||
|Start| 100/10/90| 0/0/0| +10|
|After a Tail is processed| 99/10/89| 1/1/0| +9 (-1 change)|



By doing this process (take, flip, add) for 10 coins from pile 1 to pile 2, the initial +10 advantage is eliminated and you are left with two piles with the same number of heads.

# why must you start with the initial coins all allocated to pile 1?

The puzzles' starting information is relative to the coins as a single initial group.
Because of the randomness (whether you will select a coin which starts as heads or tails from the initial group) you will never be able to tell how many heads are in the two piles at the end of the puzzle, only the ratio of heads/tails between the piles.

You only ever know the _total_ difference in heads between the starting group and *all* operations performed on it. You need to "internalise the randomness" by keeping all of the operations in one group.
Because of this you can't split the operations that you perform on the initial 100 coins into two groups. 