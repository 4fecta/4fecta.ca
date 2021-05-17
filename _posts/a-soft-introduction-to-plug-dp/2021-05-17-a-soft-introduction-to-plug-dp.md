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

In short, Plug DP is a bitmasking technique that allows us to solve complicated problems with relatively simple states and transitions. To illustrate Plug DP in its most primitive form, let's visit a rather classical problem: **How many ways can we fully tile an $$N \times M$$ grid with $$1 \times 2$$ dominoes?**

This problem can be solved with a standard row-by-row bitmasking approach, but the transitions for that DP state is annoying and unclear at best. Instead, let's investigate an approach that uses a slightly different state. Our state, $$dp[i][j][mask]$$, will represent the number of possible tilings of all cells in rows $$i-1$$ and earlier, and the first $$j$$ cells in row $$i$$, with a plug mask of $$mask$$. The first two dimensions are relatively straightforward, but what do I mean by "plug mask"?

### The Plug Mask

<figure>
<img src="fig1.jpg" alt="figure 1">
<figcaption>What the state represents</figcaption>
</figure>

Let's consider a concrete example to understand the concept of plug masks. Consider the diagram above, where the first two dimensions $$(i, j) = (3, 4)$$. The red line denotes the line which separates the cells we've already processed and the cells we have yet to consider. This line can be split into $$M+1$$ segments of length 1, and each of the arrows on these segments represent a plug. The plug itself can represent a variety of things, but for our purposes it represents whether we have placed a domino that crosses the plug (i.e. the two halves of the domino lie on separate sides of the plug). The plug will be $$1$$ (toggled) if there is a domino laid over it, and $$0$$ otherwise. For example, the diagram below depicts one of the tilings that has the plugs with states $$[1, 0, 1, 0, 1, 0, 0, 1, 0]$$ from left to right. We can obviously represent the set of states of the plugs using a bitmask of length $$M+1$$, so the DP state which the tiling below belongs to is $$dp[3][4][101010010_2]$$.

<figure>
<img src="fig2.jpg" alt="figure 2">
<figcaption>A possible tiling represented by the binary mask 101010010</figcaption>
</figure>

### Transitions

In general, we want to transition from cell $$(i, j - 1)$$ to cell $$(i, j)$$ (i.e. across each row). Notice that the only 2 plugs change locations when we move horizontally, which is the main reason why Plug DP ends up being so powerful. Specifically, if we number the plugs from $$0$$ to $$M+1$$, then only plugs $$j-1$$ and $$j$$ change locations. Specifically, $$j-1$$ goes from the vertical plug in the previous state to a horizontal plug in the next, while $$j$$ goes from a horizontal plug to the vertical plug. It is convenient to note that if we $$1$$-index the columns and $$0$$-index the plugs, then plug $$j$$ will always be the vertical plug when considering a state at column $$j$$.

<figure>
<img src="fig3.jpg" alt="figure 3">
<figcaption>Going from (3, 3) to (3, 4)</figcaption>
</figure>