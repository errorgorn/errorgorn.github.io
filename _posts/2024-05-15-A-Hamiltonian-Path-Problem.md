---
tags: math
---

## A Hamiltonian Path Problem (MA6433 Assignment)

For MA6433 (Graph Theory), one of the tasks was to create a problem.

Here is the problem I created for it: How many non-isomorphic graphs of $2022$ edges and $\binom{2021}{2}$ edges contain a Hamiltonian path?

<details><summary markdown="span">Solution</summary>

Answer: $1$

If the graph was $K_{2021} + K_1$, obviously there would not be any Hamiltonian paths since the graph is disconnected. Otherwise, we can prove that there always exists a Hamiltonian path.

The main idea is to use Ore's theorem on a subgraph of $n-1$ vertices to obtain a Hamiltonian cycle and then perform surgery to get a Hamiltonian path. The details are not very interesting.

For reference, Ore's theorem is that a simple graph with $n \geq 3$ vertices contains a Hamiltonian cycle if, for every pair of non-adjacent vertices, the sum of their degrees is at least $n$.

First, check all small graphs with $n \leq 3$. From here, assume $n \geq 4$.

Let $v$ be the vertex with the minimum degree $\Delta$. Note that $\Delta < n-1$, or else the graph is complete.

We will split into the cases:

- Case $1$: $1 \leq \Delta \leq n-3$
- Case $2$: $\Delta = n-2$

Case $1$:

After removing $v$, the graph has $n-1$ vertices and at least $\binom{n-1}{2} - (n-3)$ edges.

Suppose the condition for Ore's theorem is false and there is two non-adjacent vertices with their sum of degrees being less than $n-1$, that means the graph has at most $\binom{n-1}{2} - (2n-3) + (n-1) = \binom{n-1}{2} - (n-2)$ edges, which contradicts our previous assumption.

Choose any vertex $x_1$ such that there is an edge between $v$ and $x_1$. Since the subgraph $G-v$ contains a Hamiltonian cycle, we can pick a Hamiltonian path in $G-v$ ending with $x_1$. To get a Hamiltonian path in $G$ just append $v$ to the previous Hamiltonian path.

Case $2$:

After removing $v$, the graph has $n-1$ vertices and exactly $\binom{n-1}{2} - (n-2)$ edges. We almost satisfy the condition to use Ore's theorem.

Pick any pair of vertices $(a,b)$ without an edge and add an edge between them, so that we obtain a Hamiltonian cycle in $G-v$ with the addition of $(a,b)$. From this Hamiltonian cycle, we obtain a Hamiltonian path in $G-v$ without the addition of $(a,b)$, since at most $1$ edge (the edge $(a,b)$) of the Hamiltonian cycle should not be used. Let the resulting Hamiltonian path be $x_1x_2 \ldots x_{n-1}$.

Since $\Delta = n-2$, by the pigeonhole principle, at least one of $x_1$ and $x_{n-1}$ is connected to $v$. By appending $v$ to either the front of back of our Hamiltonian path in $G-v$, we obtain a Hamiltonian path in $G$.

</details>

Now, there is a related problem which I have not really spent time thinking about, what the is minimum number of edges required in a **connected** graph so that a Hamiltonian path always exists?