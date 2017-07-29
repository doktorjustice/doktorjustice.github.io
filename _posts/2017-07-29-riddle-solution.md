---
title: How I overvalued the spy
subtitle: Rough methods give rough solutions
date: 2017-07-29
---

The [solution for last week’s riddle](https://fivethirtyeight.com/features/pick-a-number-any-number/) came out yesterday, and it turns out not only was [my method](http://dotkomblog.com/2017/07/24/how-much-is-a-spy-worth/) rough at the edges but it completely missed the point. Now I know why.

Just as a reminder the riddle was this:

> In a distant, war-torn land, there are 10 castles. There are two warlords: you and your archenemy, with whom you’re competing to collect the most victory points. Each castle has its own strategic value for a would-be conqueror. Specifically, the castles are worth 1, 2, 3, …, 9, and 10 victory points. You and your enemy each have 100 soldiers to distribute, any way you like, to fight at any of the 10 castles. Whoever sends more soldiers to a given castle conquers that castle and wins its victory points. If you each send the same number of troops, you split the points. (...) the warlord commanding the spy has complete knowledge of the enemy’s strategy (how many soldiers will be sent to each castle). On the other hand due to some earlier battles he has much less soldiers. The question: how many soldiers is the spy worth, meaning what’s the required minimum amount of soldiers that can win the war together with the spy, no matter what the enemy does?

At least I got the investment logic (and ruling out ties) as a starting point right. Then I turned to brute force too early, but the winner has provided an elegant way of solving the puzzle. He looked for the one single enemy strategy that requires the most investment from us to gain each points: 1, 3, 5, 7, 9, 11, 13, 15, 17, 19. In this setup no matter which castle we aim for it will cost us 2 soldiers per winning points.

This is the worst setup to fight against, but still beatable __with 56 soldiers__ conquering the first 7 castles. So the spy equals the value of 44 soldiers. In any other setup there would be relatively ‘cheaper’ castles lowering the overall average ‘cost’ of getting the 28 points.

## Why did I fail?

My solution was 53 soldier based on a sample of 115 thousand seemingly viable random enemy strategies. But this is a dumb way when the actual solution depends on one single enemy strategy from the billions of possible combinations. Trying to find a needle in a haystack by picking up only the first hundred pieces is not a viable solution, though it might pay-off if it’s a state-funded project with loose quality requirements.

On the other hand in the real world it’s much more common to look for a _typical_ value or range by sampling the population or creating simulations of the combinations of input variables. My code was too simplified for that too, of course, but anyway, I had fun writing it!