---
layout: post
title:  "A Soft Introduction to Plug DP"
date:   2021-05-17 09:54:01 -0500
categories: programming
tag:
  - programming
---

So I realized that there are literally 0 resources written in English on Plug DP, which is one of my favourite DP tricks/techniques that I know of so far. Thus, I hope this serves as a soft introduction for English speakers to this technique, and maybe sheds some light on how powerful it can be. Before we start, I would recommend having a solid understanding of bitmask DP in general in order to get the most out of this blog. Now, let's begin.

### What is Plug DP?

In short, Plug DP is a bitmasking technique that allows us to solve complicated problems with relatively simple states and transitions. To illustrate Plug DP in its primitive form, let's visit a rather classical problem: **How many ways can we tile an $$N \times M$$ grid with $$1 \times 2$$ dominoes?**

<figure>
<img src="fig1.jpg" alt="figure 1">
<figcaption>What the state represents</figcaption>
</figure>
