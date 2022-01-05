---
layout: post
title:  "Path Finder Editorial"
date:   2022-01-04 12:42:22 -0500
categories: programming
tag:
  - programming
---
Hi, here's a blog post I found on my old website that I decided to transfer here. The problem being discussed is [Path Finder](https://dmoj.ca/problem/pathfind)

In order to solve this problem, you must first know at least one of the two following graph algorithms: Breadth First Search (BFS) or Depth First Search (DFS). If you are not familiar with these, I recommend doing a quick Google search to see a quick overview on how they do what they do. With that out of the way, let’s proceed to the solution.

For the first subtask, notice that the grid can have a maximum of $$2\,000 * 2\,000 = 4\,000\,000$$ cells in the grid. If we view each cell of the grid as a node and we add edges to the direct neighbours of each node, we can directly apply either BFS or DFS to the given grid. This direct application of any graph traversal algorithm has a time complexity of $$\mathcal{O}(NM)$$. Below you can see a simple implementation of this algorithm:

{% highlight cpp %}
#include <bits/stdc++.h>

using namespace std;

const int MN = 2005;

int n, m, k, vis[MN][MN], blocked[MN][MN];
pair<int, int> dir[4] = { {-1, 0}, {1, 0}, {0, -1}, {0, 1} };

void dfs(pair<int, int> cur) {
    vis[cur.first][cur.second] = 1;
    for (auto d : dir) {
        pair<int, int> nxt = {cur.first + d.first, cur.second + d.second};
        if (nxt.first < 1 || nxt.first > n || nxt.second < 1 || nxt.second > m) continue;
        if (vis[nxt.first][nxt.second] || blocked[nxt.first][nxt.second]) continue;
        dfs(nxt);
    }
}

int main() {
    cin >> n >> m >> k;
    for (int i = 1, r, c; i <= k; i++) {
        cin >> r >> c;
        blocked[r][c] = 1;
    }
    dfs({1, 1});
    if (vis[n][m]) printf("YES\n");
    else printf("NO\n");
}
{% endhighlight %}

For full marks, note that the algorithm above is simply too inefficient. It would require over $$2 \times 10^{11}$$ operations, which is far too much for any modern computer to handle in less than a second. Instead, we need to come up with a more clever application of what we already know. After re-reading the problem constraints, we notice that $$K$$ is also suspiciously low. This entices us to considering an algorithm not based on open cells, but on closed ones! Let us analyze how the patterns formed by the walls in the grid influences whether there is a path from the top-left to the bottom-right. First of all, we can assert that there is never a path when at least one “chain” of walls goes from the left edge to the right edge, the top edge to the bottom edge, the left edge to the top edge, or the right edge to the bottom edge. We can imagine these as walls running along both extreme boundaries, and are thus impassible. If no such chains exist, then we can always “walk around” each segment of walls, and we are never completely restricted by them. Thus, it is sufficient to check that there do no exist any aforementioned “chains”. Since there are only $$K$$ walls to traverse as nodes, our new algorithm has a time complexity of $$\mathcal{O}(K)$$. However, in order to efficiently store all blocked cells, we may need an array of sets, each set storing the blocked cells in the corresponding row. This will give the solution some constant, but it should still pass well under the 1 second limit. Below is an implementation of the algorithm:

{% highlight cpp %}
#include <bits/stdc++.h>
#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/tree_policy.hpp>

using namespace std;
using namespace __gnu_pbds;

typedef tree<int, null_type, less<int>, rb_tree_tag, tree_order_statistics_node_update> ordered_set;

#define ll long long
#define ld long double
#define pii pair<int, int>
#define f first
#define s second
#define readl(_s) getline(cin, (_s));
#define boost() cin.tie(0); cin.sync_with_stdio(0)

vector<pii> dir = { {0, 1}, {1, 1}, {1, 0}, {1, -1}, {0, -1}, {-1, -1}, {-1, 0}, {-1, 1} };

const int MN = 500005;

int n, m, k, r_i, c_i;
set<int> blocked[MN], vis[MN];
vector<int> a, b;
bool ans = 1, wall = 0;

void dfs(pii cur) {
    if (cur.f == 1 || cur.s == m) wall = 1;
    if (vis[cur.f].count(cur.s)) return;
    vis[cur.f].insert(cur.s);
    for (pii d : dir) {
        pii nxt = {cur.f + d.f, cur.s + d.s};
        if (blocked[nxt.f].count(nxt.s)) dfs(nxt);
    }
}

int main() {
    boost();

    cin >> n >> m >> k;
    for (int i = 1; i <= k; i++) {
        cin >> r_i >> c_i;
        blocked[r_i].insert(c_i);
        if (c_i == 1) a.push_back(r_i);
        if (r_i == n) b.push_back(c_i);
    }
    for (int r : a) {
        wall = 0;
        dfs({r, 1});
        if (wall) ans = 0;
    }
    for (int i = 0; i < MN; i++) vis[i].clear();
    for (int c : b) {
        wall = 0;
        dfs({n, c});
        if (wall) ans = 0;
    }
    for (int i = 0; i < MN; i++) vis[i].clear();
    if (ans) printf("YES\n");
    else printf("NO\n");

    return 0;
}
{% endhighlight %}
