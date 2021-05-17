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

In short, Plug DP is a bitmasking technique that allows us to solve complicated problems with relatively simple states and transitions. To illustrate Plug DP in its most primitive form, let's visit a rather classical problem: **How many ways can we tile an $$N \times M$$ grid with $$1 \times 2$$ dominoes?**

This problem can be solved with a standard row-by-row bitmasking approach, but the transitions for that DP state is annoying and unclear at best. Instead, let's investigate an approach that uses a slightly different state. Our state, $$dp[i][j][mask]$$, will represent the number of possible tilings of all cells in rows $$i-1$$ and earlier, and the first $$j$$ cells in row $$i$$, with a plug mask of $$mask$$. The first two dimensions are relatively straightforward, but what do I mean by "plug mask"?

### The Plug Mask

<figure>
<img src="fig1.jpg" alt="figure 1">
<figcaption>What the state represents</figcaption>
</figure>

Let's consider a concrete example to understand the concept of plug masks. Consider the diagram above, where the first two dimensions $$(i, j) = (3, 4)$$. The red line denotes the line which separates the cells we've already processed and the cells we have yet to consider. This line can be split into $$M+1$$ segments of length 1, and each of the arrows on these segments represent a plug. The plug itself can represent a variety of things, but for our purposes it represents whether we have placed a domino that crosses the plug (i.e. the two halves of the domino lie on separate sides of the plug). The plug will be $$1$$ (toggled) if there is a domino laid over it, and $$0$$ otherwise.

<figure>
<img src="fig2.jpg" alt="figure 2">
<figcaption>An possible tiling represented by the binary mask $$101010010$$</figcaption>
</figure>