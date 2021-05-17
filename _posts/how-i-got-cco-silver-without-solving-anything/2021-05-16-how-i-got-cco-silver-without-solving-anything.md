---
layout: post
title:  "How I got CCO silver without solving anything"
date:   2021-05-16 20:40:20 -0500
categories: programming
---
I would like to preface this by saying that you should not voluntarily attempt what I did at CCO if you are serious about your results. My strategy was mainly damage control from day 1, and it was a miracle I even got silver. However, you may find my first CCO experience interesting or maybe even hilarious, which is why I am making this post. Anyways, please enjoy. 🙂

## Day 1

### P1. Swap Swap Sort

The first fault in my approach at CCO. I had some initial ideas early on, scoring the first 2 subtasks at around 16 minutes. Over the next hour, I delevoped a solution which runs in square root log time, believing it would pass with ease due to a fatal misreading of the constraints. The solution worked well for the first three subtasks, but for some reason I couldn't figure out kept receiving a `WA` verdict on the last batch. For around another hour, I read over my code countless times and ran multiple fast-slow scripts, but simply couldn't find out where the error lied. After a gruesome 2 hour and 30 minutes on problem 1, I decided to take a peek at the rest of the problemset before it was too late (although it was already way too much time wasted).

### P3. Through Another Maze Darkly

I had a quick glance over problem 2, but decided that the subtasks from problem 3 were a lot more approachable. Really, my goal here was to snatch whatever I can and rush back to problem 1 where I had the most potential points yet to be earned. A quick program that printed the path given by a random line graph revealed the pattern of $$ 1-x-1-y-1-...-1-N-1-N-... $$, for which the first idea that came to mind was binary search. This was the smoothest subtask of day 1 for me, taking only around 20 minutes of time in total.

### P2. Weird Numeral System

This problem was intimidating at first, with no clear idea on how to proceed. Again, I was simply controlling the damage of my poor problem 1 performance here, so I was only aiming for the subtask. The subtask provided the constraint that the absolute value of any allowed digit is less than the base, which intuitively meant that any change caused by a higher base couldn't be reverted by lower bases, no matter what values we assign them. This inspired a brute force recursion in descending order of power, ensuring that the number is in the range $$ (-b^K, b^K) $$ when we are done with the $$K$$-th power (of course, $$b$$ denotes the given base here). The solution ended up running surprisingly fast (0.1 seconds), but kept getting `WA` on case 7. After around 10 minutes of debugging, I decided it was all or nothing at this point and simply slapped `__int128` into my code, which surprisingly fixed the bug and gave me the first subtask. Overall, this was around 30 minutes spent on 8 marks, which I was relatively pleased with (considering that was more than half of the points I had earned so far).

### The struggle with P1 continues

It was only here that I decided it may be a good idea to reread the statement, and discovered that contrary to my belief that $$Q = 100\;000$$, the constraints actually had $$Q = 1\;000\;000$$! A quick 1 line fix to my `MQ` constant resolved the `WA` verdict, but replaced it with the ever so agonizing `TLE` instead.
