---
tags: cp
---

## Some Speedups for Matching Algorithms

First a very silly trick.

During IAEPC, there was a problem that could be modeled as a bipartite graph with $10^6$ vertices on each side. And each vertex had like $O(\log n)$ edges or something.

Somehow maroonrk managed to squeeze TL on that.

His trick was the reverse all edges and compute the cost from sink to source.

The test case creator did not consider that and therefore the tests were weak and he managed to scam.

Now for more legitamate tricks.

### Bitset Kuhn's

We know that for unweighted bipartite matching, it is $O(VE)$ using Kuhn's and $O(E \sqrt{V})$ using Dinic's.

But actually there is a way using $O(\frac{V^3}{w})$ using bitset. It is useful when the graph is very dense. The main idea is to store the adjacent list as a bitset. Then for each left edge, we will use the bitset to check the unvisited vertices on the right side.

Although I have no idea how practical this idea is compared to using DInic's.

### Kuhn Speedup when Flow is small

Note that we only need to actually reset the visited graph from the dfs when we successfully find an augmenting path.

Therefore, the time complexity of Kuhn's is really $O(FE)$.

If we know that $F \ll V$, this can be significant speedup.

### Special Case of Weighted Bipartite Matching

Suppose a have a bipartite matching where if we have edges $L_i$ and $R_j$ in the matching, then we get an additional cost of $C_i$ (which is dependent only on the left endpoint of the matching).

It turns out we can use Kuhn's to solve this min-cost max-flow problem. And we actually perform MCMF algorithm when doing so.

The algorithm is to sort the vertices on the left side by $C_i$. Then we will attempt to push an augmenting path through each vertex on the left side from lowest to highest value of $C_i$.

If one models this as a MCMF graph, we can see that this is the strategy that MCMF will take, since the edges with cost are all directly connected to the source.

So we claim that the only viable augmenting paths are those that go through exactly $1$ edge that is incident to the source.

Suppose otherwise, and there is a viable augmenting path that uses the edges $s \to a \to \ldots \to b \to s \to c \to \ldots $.

But we know that $s \to a \to \ldots \to b \to s$ should not be a negative weight cycle from properties of MCMF.

So it is valid to take $s \to c \to \ldots $ anyways.

So each augmenting path only uses $1$ edge incident to the source. 

So the algorithm can be seen as picking the vertex on the left side with the minimum $C_i$ such that we can push an augmenting path through it.

There is another more ad-hoc way to think about this.

Suppose $C_1 \leq C_2 \leq \ldots \leq C_n$ and we know that in an optimal solution, we will pick vertices $x_1 < x_2 < \ldots < x_k$ on the left side.

We will show inductively that the optimal solution must contain the vertices $x_1 < x_2 < \ldots < x_i$.

Suppose we already know the optimal solution must contain the vertices $x_1 < x_2 < \ldots < x_i$ and the optimal solution is actually $x_1 < x_2 < \ldots  x_i < y_{i+1} < y_{i+2} < \ldots < y_k$.

Firstly, we must have $x_{i+1} \leq y_{i+1}$, because otherwise, from algorithm we know that taking $\{x_1,x_2,\ldots,x_i,y_{i+1}\}$ on the left side is impossible.

Now we will show that we can change one of the $y_j$ to become $x_{i+1}$.

Now, consider the matching $M$ when the vertices on the left side are $\{x_1,x_2,\ldots,x_i,x_{i+1}\}$ and the matching $M'$ when the vertices on the left side are $\{x_1,x_2,\ldots,x_i,y_{i+1},y_{i+2}, \ldots, y_k\}$.

Consider the union $M \cup M'$. The component containing $x_{i+1}$ is clearly a path. If the path ends on the right side of the graph, then $M'$ isn't a maximal matching because we found an $M'$-augmenting path.

Otherwise, the path ends in one of $y_{i+1},y_{i+2}, \ldots, y_k$. Since $x_{i+1} \leq y_{i+1},y_{i+2}, \ldots, y_k$, it is optimal to flip that augmenting path.

Therefore, we have two different but probably very similar ways of proving our claim.

These last two optimizations are used in [CF1728F](https://codeforces.com/problemset/problem/1728/F), if you want to solve that.
