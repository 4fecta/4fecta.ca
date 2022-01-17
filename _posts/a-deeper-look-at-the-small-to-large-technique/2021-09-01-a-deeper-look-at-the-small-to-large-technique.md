---
layout: post
title:  "A Deeper Look at the Small-to-Large Technique"
date:   2021-09-01 19:30:13 -0500
categories: programming
tag:
  - programming
---
The small-to-large technique is well known among the competitive programming community, but most problems that require it are straightforward applications of set merging. Here, we introduce a different way to think about the technique so that it may be applicable to a wider variety of problems. Consider the problem [Non-boring Sequences](https://open.kattis.com/problems/nonboringsequences). In short, the problem asks to determine whether all consecutive subsequences of a sequence $$A$$ contain a unique element (such sequences are termed "non-boring"). Surely, after a quick read, we can start to brainstorm some segment tree or other data structure based solution, but what if we try some brute force approaches as well?

Consider a recursive `check` function taking parameters $$l$$ and $$r$$ for whether the subsequence $$A[l, r]$$ is non-boring. For some $$i$$ in $$[l, r]$$, if $$A_i$$ is unique (the previous and next appearance of $$A_i$$ in $$A$$ occurs outside of $$[l, r]$$), then all subsequences of $$A[l, r]$$ "crossing" $$i$$ are non-boring. Thus, it suffices to check that both $$A[l, i-1]$$ and $$A[i+1, r]$$ are non-boring with a recursive call to `check` (note that we only need to recurse for one such $$i$$ if it exists, think about why this is). 

Naively, the algorithm above runs in $$\mathcal{O}(N^2)$$, far too slow for the given constraint of $$N \leq 200\;000$$. However, what if we try a different order of looping $$i$$ in the `check` function? Instead of looping $$i$$ in the order $$[l, l+1, l+2, .., r]$$, we will loop $$i$$ "outside-in", in the order $$[l, r, l+1, r-1, l+2, r-2, ...]$$. As it turns out, this provides us with an $$\mathcal{O}(N \log N)$$ algorithm, which is a dramatic improvement from what we thought would be $$\mathcal{O}(N^2)$$!

To prove this, consider the tree formed by our decisions for $$i$$ for each $$i$$ recursively chosen by `check`. This will be a binary tree, where the number of children in the left child is the size of the left side of our split $$[l, i-1]$$, and the number of children in the right child is the size of $$[i+1, r]$$. At each step of the algorithm, we are essentially "unmerging" a set of objects into the left and right children, giving each child the corresponding number of objects to its size. Note that this unmerging happens in a time complexity proportional to the size of the smaller child, by nature of us looping outside-in. However, considering the reverse process, this is exactly the process of small-to-large set merging, which is $$\mathcal{O}(N \log N)$$! Thus, we have obtained the correct complexity of our algorithm, and this problem is solved with barely any pain or book-code. Below shows a C++ implementation of `check`, where `lst` and `nxt` store the index of the previous and next appearance of $$A_i$$ respectively:

{% highlight cpp %}
bool check(int l, int r) {
    if (l > r) return 1;
    for (int i = l, j = r; i <= j; i++, j--) {
        if (lst[i] < l && nxt[i] > r) return check(l, i - 1) && check(i + 1, r);
        if (lst[j] < l && nxt[j] > r) return check(l, j - 1) && check(j + 1, r);
    }
    return 0;
}
{% endhighlight %}

In conclusion, it may be worth the time to consider seemingly brute force solutions to some problems, as long as there is a merging or unmerging process that can happen proportional to the size of the smaller set, capitalizing on the small-to-large technique when it seems like the last thing one could do.
